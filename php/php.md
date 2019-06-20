# php

## php 基本用法

### 引用外部php文件

```
require_once('xxx');
include_once('xxx');

```

二者区别: include_once 在引用文件发生错误的时候会继续向下执行，`require_once` 引用文件发生错误时不会向下执行 

### array

声明一个空数组: `$arr = array();` 

声明数组: `$arr = array(''xxx' => 'sss', 1 => 'sess')` ；

push 方法： \`array_push($arr, value);

### session机制

$\_SESSION 相当于是一个全局变量，可全局访问;

用法: 在 a.php 中执行 `session_start();` 方法 然后执行 `$_SESSION['xxx]' = xxx;` 设置 session;

在其他文件访问时，同样需要执行 `session_start()` 方法 然后执行 `$_SESSION['xxx']` 读取变量

### 声明变量

局部变量：`$xxx = 'sss'` ;

全局变量：`$GLOBAL['xxx'] = 'ssss';`

在php 函数中不能访问函数外声明的变量 要是用 `global` 关键字: `global $a；`

## 前后端交互

当前端页面发来请求的时使用 `$_GET['xxx']` 获取 get 请求的字段，使用 `$_POST['xx']` 获取 post 请求的字段；

使用 `$_REQUEST['xx']` 两种请求都能获得

### 设置响应头信息

`header(Content-type: 'application/json; charset=utf-8')` 

## mysql 数据库

实现增删改查

增：`"INSERT INTO tablename (column1, column2, column3, ....)VALUES(value1, value2,value3,...)"` 

删: `"DELETE from tablename where column_name = xxx"` 

改：`"UPDATE tablenames SET column1 = '$a', column2 = '$b', .... where columnxx = 'xx' AND columnYY = 'xx'"` 

查：`"SELECT *from tablename"`  

链接数据库:

```
$con = mysqli_connect(server, username, password, database);
// 设置数据库的编码，防止乱码 
mysqli_set_charset($con, 'utf8')

```

查询数据库内容:

```
$sql = "SELECT *from tablename" 
$row = mysqli_query(sql);

```

上面的语句会获取表中的所有数据

```
while($row = $rows -> fetch_assoc()) {
    array_push($arr, array('column': $row['columnname']),...)
}

```

## php 面向对象编程

#### 面向对象软件开发流程：

分析--->设计 --> 编程

#### 面向对象的目标

重用行，灵活性，扩展性

#### 面向对象语言的特点

封装，继承，多态

#### 类主要特性

属性，方法，标示

#### 对象主要特性

行为，属性（状态），标识

#### 构造方法

对象创建的时候运行

```
function __construct() {
}

```

#### 析构方法

对象不在运行的时候运行

```
function __destruct() {
}

```

### 封装性

public(公有的), private（私有的，只能在类内调用，不能被继承）, protected（在内调用，可以被继承）

#### 魔术方法

##### \_\_set

只针对被保护的变量,设置变量的时候被调用

\_\_get

外部调用时方法被调用

\_\_isset

isset 方法被调用时调用

\_\_unset

unset方法调用时被调用

```
class Person {
  public function __set($key,$value) {
    if ($key == 'age' && $value == 28) {
      $this -> name = 'xiangwang';
    }
  }
  
  public function __get($key) {
    if ($key == 'age') {
      return $this -> age; 
    }
  }
  
 public function __isset($key) {
    if ($key == 'age') {
      return 0;
    }
 }
 
 public function __unset($key) {
  if ($key == 'age') {
    unset($this ->age);
  }       
}

}

```

### php 对象的继承和多态

重载，访问控制，继承

子类中重写可以覆盖父类的方法

子类中通过 parent:: fuc 调用被覆盖的方法

子类中重新声明富类中的方法，父类的方法回重写。

protected 可以在子类中被访问，外面不能访问

private 类型 只能在 自己类中被访问。

```
<?php
  class Person {
      public $name;
      private $age;
      protected $money;
      function __construct($name, $age, $money) {
          $this -> name = $name;
          $this -> age = $age;
          $this -> money = $money;
        
      }

      public function cardInfo() {
          echo $this -> name.$this -> age.$this->money;
      }

  }

  class Yellow extends Person {
    // function __construct ($name, $age, $money) {
    //     parent:: __construct($name, $age, $money);
    // }

    public function cardInfo() {
        parent::cardInfo();
    }

    public function text() {
        echo $this -> money;
    }
  }

  $s = new Yellow('qqq', 23, 100);

  $s-> cardInfo();

  echo $s -> name;

//   echo $s -> age; // error
//   echo $s -> money; // error

  $s -> text(); // 100
?>

```

### 抽象类与接口

抽象类只能被继承不能被实例化，抽象类中的抽象方法，必须在子类中被实现。抽象方法只能放在抽象类中，抽象类中可以没有抽象方法

抽象方法:

`public abstract function func()` 

抽象类

#### 接口

一个类可以继承多个接口,只能继承一个类

接口中的数据类型为静态的

静态的需要 使用`实例::静态值` 或者直接使用`类:: 静态值或者静态方法` 进行访问，同样收到 private public 约束；

定义动作，不干具体的活

```
    /* 抽象类
    abstract class Person{
        public abstract function eat();    
    }



    class Man extends Person{
        public function eat (){
            echo 'eat';
        }
    }

    $man = new Man();

    $man->eat();
    */

    // 接口可以声明常量
    // 借口中的方法都是抽象方法，不用abstract 去定义
    // 接口特不能被实力化，需要一个类来继承
    interface Person {
        const name = 'wwwww';
        public function run();
        public function eat();
    }

    interface Study {
        public function study();
    }

    class Student implements Person,Study {
        public function run() {
            echo 'run';
        }

        public function eat() {
            echo 'eat';
        }

        public function study() {
            echo 'study';
        }
    }

    $x = new Student();

    $x->eat();

    echo $x::name

```

#### 命名空间

不同的命名空间可以声明相同的变量

#### js 继承的实现

声明一个类:

```
function Car(color) {
    this.color = color;
}

Car.prototype.run =  function() {
    console.log(this.color+'run');
}

```

上面就是声明了一个类

new Car('red'); 返回 

<img src="https://dev.seafile.com/seahub/lib/6cf9337f-ea7c-4c6c-8bc2-94bf97971a68/file/images/auto-upload/image-1560875436650.png?raw=1" width="243" height="null" />

通过 Car.prototype 将方法挂到原型上，就是父类上,也就是挂带原型上，挂到原型上之后，所有的实例都可以使用该方法，也即 prototype 上的方法就是 父类上的方法

js对象中的方法是通过原型链一层一层的往上找的

声明一个子类:

```
function Cruze(color) {
    Car.Call(this, color);// 相当于在 Cruze 中执行 this.color = color;
}

// 得到一个 Car 的实力作为 Cruze 的原型
__prototype = Object.create(Car.prototype);
 // 将 contructor 写为 Cruze; 
 __prototype.constructor = Cruze;
 // 写入原型链

Cruze.prototype = __prototype;

```

js 的继承是根据原型链完成继承

#### sql 语句

连接本地的mysql

`mysql -uroot -p` ;

显示所有数据库

showdatabases;

创建数据表

```sql
CREAT TABLE `new_schema`.  `t_student`(
`id` INT NOT NULL AUTO_INCREMENT COMMENT '',
 `name` VARCHAR(45) NOT NULL,
 .....
 .....    


)

```

```
SELECT * FROM new_schema.t_student

SELECT * from new_schema.t_student where id=3
SELECT count(*) from new_schema.t_student where id=3

SELECT gender from new_schema.t_student where id=3
SELECT count(*) from new_schema.t_student where id=3

select min(birthdate) from new_schema.t_student
select min(birthdate) from new_schema.t_student

// 获得生日最小值的所有键值
select min(birthdate),t_student.* from new_schema.t_student
select max(birthdate),t_student.* from new_schema.t_student LIMIT 0, 1000

select now()

select round(rand()*100)

select concat(id, '---', name) from t_student


select * from t_student where birthdate>='1990-01-01' and birthdate <= '1999-12-31' 

select gender from t_student where birthdate>='1990-01-01' and birthdate <= '1999-12-31' 
select * from t_student where birthdate between '1990-01-01' and '1999-12-31' 
select * from t_student where name like '%四%'
select * from t_student order by birthdate 
select * from t_student order by birthdate desc

// 按照班级拼接 （表的拼接）

select * from t_student,t_class where t_student.class=t_class.class;

select t_student.id,t_student.name, t_class.name from t_student,t_class where t_class.class=t_student.class

SELECT * FROM t_class1,t_student where t_student.class=t_class1.class



```
