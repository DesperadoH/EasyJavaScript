## 第一章 数据类型

### 1.六种数据类型

var num = 32;

num = 'this is a string'

32 + 32 //64

五中原始类型 一种对象类型

原始类型: number string boolean null undefined


object : Function Array Date ...

### 2.隐式转换

var x = 'string' + 42 

'37' - 7 //30   -号理解为减法运算

'37' + 7 //377  +号理解为字符串拼接

巧用+/- 规则转换类型

num - 0 转为数字 , num + '' 转为字符串

"1.3" == 1.3  

null === null

undefined === undefined

NaN !== NaN

[1,2] == [1,2]   new Object == new Object

[1,2] === [1,2]  //false 对象是引用去比较他们俩不是相同一个对象

new Object !== new Object


### 3.包装对象

在原始类型中 number string boolean 这三种原始类型都有对应的包装类型

var str = 'string'  ------基本类型

var strObj = new String('string')

str这样一个基本类型怎么会有length属性呢？

str.t = 10 

alert(str.t)  //undefined

JS中 'str'-----> String Object 

当访问完了length之后 这个临时对象会被销毁

所以str.t = 10 赋值以后再去 alert(str.t) 是undefined

除了string之外  number boolean也一样


### 4.类型检测

    typeof
    instanceof
    Object.prototype.toString
    constructor
    duck type

#### **** 1.typeof ****

typeof null === 'object' 历史原因

typeof 在判断基本类型和函数对象时候比较方便

#### **** 2.instanceof ****

对于其他类型判断

比如判断一个对象类型是不是数组 typeof 会返回 object

所以用 obj instanceof Object 基于原型链

左边的左操作数的对象 的原型链上 是否有右边的构造函数的prototype对象属性

    [1,2] instanceof Array // true
    new Object() instanceof Array // false

    function Person(){}  //undefined
    function Student(){}  //undefined
    Student.prototype = new Person() //Person
    Student.prototype.constructor = Student //function Student(){}

    var bosn = new Student()   //undefined
    bosn instanceof Student    //true

    var one = new Person()     //undefined
    one instanceof Person      //true
    one instanceof Student     //false

    bosn instanceof Person     //true
    bosn 有的原型 bosn_proto_指向 


他的构造器也就是Student.prototype这样一个属性

当判断bosn instanceof Person的时候 bosn的原型也就是Student.prototype不等于Person.prototype于是沿着原型链向上查找，于是：

bosn._proto_._proto_指向 Person.prototype 因此返回true


判断对象是否相当 JS是按照引用来判断，所以两个完全相同的空对象和空对象之间

因为不是同一个对象所以也不会相等，所以使用 instanceof 有个坑：

不同的 window 或 iframe 之间的对象检测不能使用instanceof

看起来相同的对象 在完全不同的window下是两个对象，在跨 iframe 是不可以用的



#### **** 3.Object.prototype.toString ****


    Object.prototype.toString.apply([]) ; === '[object Array]' ;
    Object.prototype.toString.apply(function(){}) ; === '[object Function]' ;
    Object.prototype.toString.apply(null) ; === '[object Null]' ;
    //  IE6/7/8 Object.prototype.toString.apply(null) ; === '[object Object]' ;

    Object.prototype.toString.apply(undefined) ; === '[object Undefined]' ;


#### **** 4.constructor ****

任何一个对象都有一个constructor属性实际上是继承自原型的
会指向构造这个对象的构造器或者构造函数，constructor 可以被修改要小心


#### **** 5.duck type ****

鸭子类型：比如不知道这个对象是不是数组，那么可以判断他的length是不是数字

他是否有join push 等等这样的方法，通过判断这些特征来判断是否属于数组

#### **** 总结 ****

typeof 适合基本类型和function检测，遇到null失效

那么怎么判断null呢 可以用严格等于null的方式来判断某个变量是否为null  m === null


通过obj.prototype.toString拿到，适合内置对象和基元类型，遇到null和undefined失效（IE6/7/8等返回[object Object])


instanceof

适合自定义对象，看是否属于构造器构造的，或者是否有这样的继承关系,在判断数组啊

正则等对象类型判断下比较好用，也可以用来检测原声对象，在不同的

iframe 和 window 间检测时失效
























