title: PHP之《深入PHP面向对象、模式与实践》
date: 2018/10/3
categories: 
- 图书
- 深入PHP面向对象、模式与实践
tags: 
- PHP
- 图书
---

# 对象基础  

本章主要内容有
- 类和对象:声明类及实例化对象
- 构造方法:自动加载对象
- 基本数据类型和类的类型:
- 继承:为什么需要继承和如何是用继承
- 可见性:整合对象接口并保护类中的方法和属性不受干涉

## 类和对象

### 编写第一个类

```php
class ShopProduce {
    // todo
}
```
简单的说,类是用于生成对象的代码模板.使用class关键字和一个任意的类名来声明类.类名可以是任意数字和字母的组合,但不能以数字开头.和一个类关联的代码必须用大括号括起来.

### 第一个对象(或两个)

如果类是声明对象的模板,那么对象是根据类中定义的模板所构造的数据.对象可以被说成类的"实例",它是由类定义的数据类型.

```php
<?php
$product1 = new ShopProducee;
var_dump($product1); // 可以看到包含对象ID的字符串
```

## 设置类中的属性

> 类的属性和标准的变量很相似,不过必须在声明和赋值前加一个代表可见性的关键字.这个关键字可以是public,protected或private,它决定了属性的作用域.

类可以定义被称为属性的特定变量.属性也称为成员变量,用来存放对象之间互不相同的数据.

虽然php并没有强制所有的属性都要在类中声明,但是那种动态地增加属性到对象的方式并不值得推荐.比如:
- 特定意义不明显,难以及时发现属性中语法之外的错误
- 类太过松散,因为属性都是在客户端代码中实现的
- 不得不重复做出一些经常做的事情

## 使用方法

> function关键字在方法名之前.方法名之后的圆括号是可选的参数列表.方法体用大括号括起来

属性可以让对象存储数据,类方法则可以让对象执行任务.方法是类中声明的特殊函数.

## 参数和类型

数据类型决定了PHP代码中处理数据的方式----从更高层次的意义上来说,每个类都定义一种数据类型.

在PHP中,定义方法和函数时不需要指定参数的数据类型,在便利的同时,也带来了一些隐患.在需要特殊指定类型的场合,这种便利会带来一些隐患

### 基本类型

数据的类型决定了PHP代码中处理数据的方式.比如,使用字符串类型显示字符数据并用字符串函数处理这些数据等等.这些类型被称为**基本类型**.
从更高层次上来说,每一个类都定义了一种数据类型.因此ShopProduct对象属于基本类型,但同时也属于ShopProduct类这一类型.

#### 基本数据类型
需要时刻留意代码中的数据类型,比如"false"和false两者结果相反的解析可能会导致截然不同的结果.解决这种问题的方法总结如下
- 检测类型
- 转换类型
- 依赖良好清晰的文档(无论哪种模式,都需要这个)
无论如何解决这类问题,都要认真考虑一件事情:类型处理.PHP是一种弱类型的语言.我们不能依赖编译器来防止类型相关的bug,必须考虑当非法数据类型的参数传递给方法是,会产生什么样的后果.我们不能完全信任客户端程序员,应始终在方法中处理他们引入的无用信息.

#### 获得提示:对象类型

```php
public function write(ShopProduct $shopProduct) { 
    // todo
}
```

最新的php7也可以把string之类东西作为类型提示.刚刚说的处理这类问题需要*检测类型*,*转化类型*或*良好的文档*,现在就会通过直接指定解决了.

需要注意的是,虽然自动类型检查是防止bug的极佳途径,但是自动类型检查实在代码运行的时候才能生效.

数据类型和类确有很多相似之处,他们有一个关键的不同之处:定义一个类也就定义了一个类型,但是一个类型可以用于描述一个家族的众多类.
这种将不同的类放在一个类型之下的机制被称为继承.

## 继承

继承是从一个基类得到一个或多个派生类的机制

继承自另外一个类的类被称为该类的子类.子类将继承父类的特性.这些特性由属性和方法组成.子类可以增加父类之外的新功能.

### 继承问题  

### 使用继承

创建继承的第一步是找到现有的基类元素中不适合放在一起,或者不需要进行特殊处理的类方法.

#### 构造方法和继承

在子类中定义构造方法是,需要传递参数给父类的构造方法,否则得到的可能是一个构造不完整的对象.
要调用父类的方法,首先要找到一个引用类本身的途径:句柄(handle).PHP在这里提供了parent关键字.
要引用一个类而不是对象的方法,可以使用::而不是->

#### 调用被覆写的方法

parent关键字可以在任何覆写父类方法中使用.覆写一个父类的方法是,并不希望删除父类的功能,而是扩展它,通过在当前对象中调用父类的方法可以达到这个目的.

### public private protected:管理类的访问

类中的元素可以被声明为public private protected
- public:在任何地方都可以访问
- private:只能在当前类中才能访问,即使是子类也不可以
- protected: 可在当前类及其子类中访问,外部代码无权访问 

# 高级特性

本章的主要内容有:
- 静态方法和属性:通过类而不是对象来获取数据和功能
- 抽象类和接口:设计和实现相分离
- 错误处理:异常
- Final类和方法:限制继承
- 拦截器方法:自动委托
- 析构方法:对象销毁前的清理工作
- 克隆对象:创建对象的副本
- 把对象解析成字符串:创建摘要型方法
- 回调:用匿名函数为组件添加功能

## 静态方法和属性

我们不仅可以通过对象访问方法和属性,还可以通过类来访问它们.这样的方法和属性是"静态的",必须使用static关键字来声明

静态方法是以类为作用域的函数.静态方法不能访问这个类中的普通属性,因为那些属性属于一个对象,但可以访问静态属性.如果修改一个静态属性,那么这个类的所有实例都能访问到这个新值.
因为通过类而不是实例来访问静态元素,所以访问静态元素时不需要引用对象的变量,而是使用::来连接类名和属性或者类名和方法.

> 只用使用parent关键字调用方法时,才能对一个非静态方法进行静态形式的调用.其他任何情况都需要先生成一个对象,然后使用->符号调用对象中的方法.
也就是说,除非访问一个被覆写的方法,否则永远只能用::访问被明确声明为static的方法或属性.

## 常量属性

虽然常量属性是公共的,可静态访问的,但客户端代码不能改变它们.

常量属性只包含基本数据类型的值。不能将一个对象指派给常量。像静态属性一样，只能通过类而不能通过类的实例访问常量属性。

## 抽象类

抽象类不能直接实例化。抽象类中只定义或部分实现子类需要的方法.子类可以继承它并且通过实现其中的抽象方法,使抽象类具体化.

抽象类的每个子类都必须实现抽象类中的所有抽象方法,或者把他们自身也声明为抽象方法.扩展类不仅仅负责简单实现抽象类中的方法,还必须重新声明方法.新的实现方法的访问控制不能比抽象方法的访问控制更加严格.新实现方法的参数个数应该和抽象方法的参数个数一样,重新生成对应的类型提示.

## 接口

抽象类提供了具体实现的标准,而接口(interface)则是纯粹的模板.接口只能定义功能而不包含实现内容.接口可用关键字interface声明.接口可以包含属性和方法声明,但是方法体为空. 

## 延迟静态绑定:static关键字

```php
<?php

abstract class DomainObject
{
    public static function create()
    {
        return new static(); // #1 return new self();
    }
}

class User extends DomainObject
{
}
```

self指的不是调用上下文,而是解析上下文.所以,上代码中的#1的static如果换成self就会出错.因为这里的self被解析成create()的DomainObject,而不是解析为调用self的User

static类似于self,但它指的是被调用的类而不是包含类.在本例中,它的意思是调用User::create()将生成一个新的User对象,而不是试图实例化一个DomainObject对象.

static关键字不仅仅可以用于实例化.和self及parent一样,static还可以用作静态方法调用的标识符,甚至是从非静态上下文中调用.

假设想为DomainObject引入组的概念.默认所有类属于default级别,同时也可以为继承层次结构的某些分支重写类别.
```php
<?php

abstract class DomainObject
{
    private $group;

    public function __construct()
    {
        $this->group = static::getGroup();
    }

    public static function create()
    {
        return new static();
    }

    static function getGroup()
    {
        return 'Default';
    }
}

class User extends DomainObject
{
}

class Document extends DomainObject
{
    static function getGroup()
    {
        return "Document";
    }
}

class SpreadSheet extends Document
{

}

print_r(User::create());
print_r(SpreadSheet::create());
```

## 错误处理

通常我们喜欢封装好多类是完整和独立的,不需要从外部干预内部代码的执行.所以依赖程序员另外写代码来测试一个类中的方法是否出错,是非常不合理的.

### 异常

异常时从PHP5内置的Exception类(或其子类)实例化得到的特殊对象.Exception类型的对象用于存放或报告错误信息.
Exception类的构造方法接受两个可选的参数:消息字符串和错误代码.其提供了一些有用的方法来分析错误条件:

**Exception类的public方法**

| 方法 | 描述
| --- | --- |
| getMessage() | 获得传递给构造方法的消息字符串 |
| getCode() | 获得传递给构造方法的错误代买 |
| getFile() | 获得产生异常的文件 |
| getLine() | 获得产生异常的行号 |
| getPrevious | 获得一个嵌套的异常对象 |
| getTrace() | 获得一个多维数组,这个数组追踪导致异常的方法调用,包含方法,类,文件和参数数据 |
| getTraceAsString() | 获得getTrace()返回数据的字符串版本 |
| __toString() | 在字符串中使用Exception对象时自动调用.返回一个描述异常细节的字符串 |

### 异常的子类化

```php
<?php

class XmlException extends Exception
{
    private $error;

    function __construct(LibXMLError $error)
    {
        $shortfile = basename($error->file);
        $msg = "[{$shortfile}, line {$error->line}, col {$error->column}]{$error->message}";
        $this->error = $error;
        parent::__construct($msg, $error->code);
    }

    function getLibXmlError()
    {
        return $this->error;
    }
}

class FileException extends Exception
{
}

class ConfException extends Exception
{
}

class Conf
{
    function __construct($file)
    {
        $this->file = $file;
        if (!file_exists($file)) {
            throw new FileException("file '{$file}' does not exist'");
        }
        $this->xml = simplexml_load_file($file, null, LIBXML_NOERROR);
        if (!is_object($this->xml)) {
            throw new XmlException(libxml_get_last_error());
        }
        echo gettype($this->xml);
        $matches = $this->xml->xpath("/conf");
        if (!count($matches)) {
            throw new ConfException("could not find root element: conf");
        }
    }

    function write()
    {
        if (!is_writable($this->file)) {
            throw new FileException("file '{$this->file}' is not writeable'");
        }
        file_put_contents($this->file, $this->xml->asXML());
    }
}

class Runner
{
    static function init()
    {
        try {
            $conf = new Conf(dirname(__FILE__) . "conf01.xml");
            echo "user:{$conf->get('user')}\n";
            echo "user:{$conf->get('host')}\n";
            $conf->set('pass', 'newpass');
            $conf->write();
        } catch (FileException $e) {
            // todo 文件权限问题或者文件不存在
        } catch (XmlException $e) {
            // todo XML文件损坏
        } catch (ConfException $e) {
            // todo 错误的XML文件格式
        } catch (Exception $e) {
            // todo 后备捕捉器
        }
    }
}
```

## Final类和方法

final关键字可以终止类的继承.final类不能有子类,final方法不能被覆写.


## 使用拦截器

**拦截器方法**

| 方法 | 描述 |
| --- | --- |
| __get($property) | 访问未定义的属性时被调用 |
| __set($property, $value) | 给未定义的属性赋值时被调用 |
| __isset($property) | 对未定义的属性调用isset()时被调用 |
| __unset($property) | 对未定义的属性调用unset()时被调用 |
| __call($property, $arg_array) | 调用未定义的方法时被调用 |

## 析构方法

在对象被垃圾收集器收集之前(即对象从内存中删除之前)自动调用.可以使用这个方法进行最后必要的清理工作.

## 使用__clone()复制对象

在对象调用clone时,可以通过__clone()实现控制复制什么.

在复制对象属性时只会复制引用,并不会复制引用的对象.
如果不希望对象属性(对象的属性是其他对象)在复制之后被共享,则可以显示地在__clone()方法中复制指向的对象.
```php
function __clone() {
    $this->id = 0; // 控制复制属性
    $this->account = clone $this->account; // 复制对象属性(不这样做会引用该对象属性)
}
```

## 定义对象的字符串值

## 回调 匿名函数和闭包

虽然匿名函数不是严格意义上的面向对象的特性,但是它非常有用.

利用回调,可以在运行时将与组件的核心任务没有直接关系的功能插入到组件中.有了组件回调,就可以在不知道上下文的情况下扩展他人的代码.

```php
<?php

class Product
{
    public $name;
    public $price;

    function __construct($name, $price)
    {
        $this->name = $name;
        $this->price = $price;
    }
}

class ProcessSale
{
    private $call_backs;

    function registerCallBack($call_back)
    {
        if (!is_callable($call_back)) {
            throw new Exception("callback not callable");
        }
        $this->call_backs[] = $call_back;
    }

    /**
     * 回调方法
     * @param $product
     */
    function sale($product)
    {
        echo "{$product->name}:processing\n";
        foreach ($this->call_backs as $call_back) {
            call_user_func($call_back, $product);
        }
    }
}

class Mailer
{
    public function sendMail($product) {
        echo "Send Mail {$product->name}";
    }
}

$logger = function ($product) {
    echo "logging {$product->name}\n";
};

/**
 * 闭包
 * Class Totalizer
 */
class Totalizer{
    static function warnAmount($amt) {
        $count = 0;
        return function ($product) use ($amt, &$count) {
            $count += $product->price;
            echo "\n{$product->name} is {$product->price}\n";
            if ($count > $amt) {
                echo "High price reached {$count}\n";
            }
        };
    }
}

$process = new ProcessSale();
try {
    $process->registerCallBack($logger); // 回调 匿名函数
    $process->registerCallBack([new Mailer(), 'sendMail']); // 回调 对象方法 #1
    $process->registerCallBack(Totalizer::warnAmount(3)); // 回调 静态方法 闭包
} catch (Exception $e) {
    die($e->getMessage());
}
$process->sale(new Product('shoes', 6));
echo "\n";
$process->sale(new Product('pen', 12));
```

注意在上述代码中的#1出的用法

这里传递了一个数组,其第一个元素是对象,第二个元素是对象中的方法名(字符串形式).is_callbale可以智能检测这类数组.另外,这种写法在array_walk等中也常见

# 对象工具

本章主要内容:
- 包:将代码按逻辑分类打包
- 命名空间
- 包含路径:为类库代码设置访问路径
- 类函数和对象函数:测试对象,类,属性和方法的函数
- 反射API:一组强大的内置类,可以在代码运行时访问类信息

# PHP和包

# PHP包和命名空间

## 命名空间

命名空间是一个容器,他可以将类 函数和变量放在其中.在命名空间中,可以无条件访问这些项.在命名空间之外,必须导入或引用命名空间,才能访问它所包含的项.

## 自动加载

__autoload()方法是一种根据类和文件的结构,管理类库文件包含的有效方法.

一个文件定义一个类,一一对应,便于管理.这种方法虽然会带来额外的开销(包含文件会带来开销),但这种组织方式特别有效.尤其是系统需要在运行时使用新类的时候.

#  类函数和对象函数

在PHP代码运行时可能无法知道正在使用的类是哪个,这是因为php中类是可以通过字符串动态加载到,而字符串是不固定的.

在实际开发中,需要先检查类是否存在,它是否拥有讲课使用的方法等,以避免可能存在的风险.

## 查找类

class_exists()函数接受表示类的字符串,检查并返回布尔值.

print_r(get_declared_classess())可以列出用户定义的类和php内置的类.注意它只返回在函数调用时已声明的类.可以继续运行require等增加脚本中类的数目.

## 了解对象或类

[get_class](http://php.net/manual/zh/function.get-class.php)

返回对象实例 object 所属类的名字。

[instanceof](http://php.net/manual/zh/language.operators.type.php) 

用于确定一个 PHP 变量是否属于某一类 class 的实例

## 了解类中的方法

get_class_method 



# 对象与设计

本章内容:
- 设计基础:设计是什么?面向对象设计和过程式编程有什么不同?
- 类的内容:如果决定一个类应该包含什么
- 封装:在类接口后面隐藏实现和数据
- 多态:使用一个共同的父类,允许在运行是透明地替换特定的子类
- UML

# 什么是设计模式?为什么使用它们

本章内容:
- 模式基础:什么是设计模式
- 模式结构:每个设计模式的关键元素
- 模式收益:为何模式值得花时间学习

# 模式原则

本章内容:
- 组合:如果通过聚合对象来获得比只是用继承更好的灵活性
- 解耦:如何降低系统中元素的依赖性
- 接口的作用:模式和多态
- 模式分类:模式类别

# 