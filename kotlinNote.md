基本类型:
=====

**数字**

Double  Float  Long  Int  Short  Byte

没有隐式拓宽转换，但算数运算可转换

数字面值可以用下划线划分
可空化、泛型会把数字装箱
装箱后相等（==）但不同一（===）
位运算采用中缀方式

**字符**
Char
没有隐式拓宽转换
可空化会被装箱，不会保留同一性

**布尔值**
Boolean
需要可空引用时会被装箱

**数组**
Array
有get、set、size方法
arrayOf 、arrayofNulls 、Array创建数组
类型不可变
原生类型数组无装箱开销

**字符串**
String
可用[]、for-in访问字符
原声字符串用 ”””
字符串中直接输出 $a、${a+b}



控制流:
----

**if**
if是一个表达式，即它会返回一个值
if else 代替 ? :
if的分支可以是代码块，最后的表达式作为该块的值

**when**
代替switch， case ->  ()
可以当作语句、表达式使用
多个分支条件若可用逗号分隔作同样处理
可以用任意表达式（而不只是常量）作为分支条件
可以用 in 、!in 作为分支条件

**for**
for in可以循环遍历任何提供了迭代器的对象
不可直接遍历数组

**while**
while 和 do..while 照常使用
Break 与 Continue 标签
任何表达式都可以用标签（label）来标记
比如 loop@ for(… ，可用break标签：break@loop

**return 标签**
可指定标签，返回指定处
从 lambda 表达式中返回可以用隐式标签
可返回值 如:return @a 1



类和继承:
-----

**构造函数**
主构造函数紧跟类名
若不声明，默认会有public不带参数的主构造函数
有注解或可见性修饰符，constructor不可省略
次构造函数需要调用主构造函数

**继承**
所有类都有一个共同的超类 Any
类默认final不可继承，加open才可继承
继承使用冒号
内部类中访问外部类的超类：super@Outer

**覆盖方法**
基类必须声明open且子类必须声明override
override方法本身为open，可以加final禁止覆盖
接口及接口中方法、抽象类及抽象方法默认为open
super<Base>区分超类型

**覆盖属性**
基类必须声明open且子类必须声明override
var属性可以覆盖val属性，反之不行
伴生对象
没有静态方法，伴生对象代替

**属性和字段:**
var表示可变的，val表示不可变的
var可设置get、set方法，val可设置get方法
kotlin1.1后get方法可推导出类型，不用声明
属性的get、set方法可用private等改变可见性
属性的get、set方法中可用filed访问幕后字段
若幕后字段filed不使用，可用幕后属性
const只能形容val，且不能有set、get方法
lateinit可延迟初始化属性

**接口:**
可有抽象方法和实现方法
属性必须为抽象属性或提供访问器，且没有幕后字段filed

可见性修饰符:
-------

private 类内部可见
protected 类内部及子类可见
internal 类声明的本模块内任何客户端可见
public 类声明的任何客户端可见
不修饰时默认为public
外部类不能访问内部类 private成员
protected成员被覆盖后默认还为protected
修饰主构造函数可见性时不能省略constructor关键字
局部变量、局部函数和局部类不能有可见性修饰符

扩展:
---

扩展类功能无需继承或装饰
伴生对象也可扩展函数和属性
一般在顶层定义扩展
扩展函数和属性可以声明为open，子类中可覆盖

**扩展函数：**
扩展函数内部this指被扩展者即扩展接受者
若同名，成员函数会覆盖扩展函数
可以为可空接受者定义扩展
没有实际函数插入类中

**扩展属性：**
没有实际函数插入类中，无幕后字段
扩展属性不能有初始化器
扩展接受者和分发接收者：
A类中扩展B类，A叫分发接受者，B叫扩展接受者
扩展函数中可直接调用A、B类中方法
若重名则优先调用B即扩展接受者
扩展函数中this指扩展接受者
扩展函数中调用分发接受者使用this@A 限定符


数据类:
----

data修饰，主构造函数至少一个参数
若要有无参构造函数，则主构造函数使用默认值
主构造函数参数必须用var或val修饰、
copy函数

密封类:
----

sealed修饰，存放有限集中的类型（类似于枚举）
子类必须在同文件中,子类的子类可以不放在同文件中
不能实例化，构造函数默认private
结合when表达式方便覆盖所有情况

泛型:
---

只能读取对象的叫生产者，只能写入的对象叫消费者
T : U 表示T上界为U类型，默认上界为Any?

**声明处型变**
out T表示只能生产返回，in T表示只能消费写入

**类型投影**
out Any相当于 ? extends Object
in String相当于 ? super String

**星投影**
对于 < in T,out U >,< *,* >表示< in nothing，out Any? >

嵌套类和内部类:
--------

嵌套类声明在其他类中，不可访问外部类成员
内部类声明在其他类中，且用inner修饰，可访问外部类成员
内部类持外部类引用，嵌套类不持有外部类引用

枚举类:
----

用于实现类型安全的枚举
每个枚举常量都是一个对象，逗号隔开
枚举常量可以实现自己的匿名类
可用valueOf和values访问枚举常量
可用enumValues<T>和enumValueOf<T>访问枚举常量

对象表达式和对象声明:
-----------

**对象表达式：**
对象表达式object : A代替匿名内部类的实现
对象表达式中可以访问外部非final的变量

**对象声明：**
object A{...}表示A为单例类，可以继承子其他类
局部作用域(函数内部)不能对象声明
类内部用companion object A对象声明为伴生对象
伴生对象成员可通过类名.方法名调用，可作为静态方法使用
伴生对象名A可省略，引用时使用名称Companion


**对象表达式和对象声明：**
对象表达式立即初始化，对象声明第一次访问时初始化
伴生对象在加载类时初始化

类委托:
----

很好的替代继承的方案
class A(b: B) : B by b,A调用B的方法会转发给b

委托属性:
-----

格式： var/val ‘属性名’: ‘类型’ by ‘表达式’
get、set方法被委托给委托的getValue、setValue方法
属性的委托需要提供getValue、setValue方法
val属性的委托只需提供getValue
延迟属性委托 by lazy
可观察属性委托 by Delegates.observable()
截获赋值委托 by vetoable
从map中取值委托 by map
用provideDelegate扩展委托逻辑



