## 表达式与运算符

### 表达式expressions

表达式是指能计算出 值 的任何可用成语单元 ——wiki

表达式是一种JS短语，可让JS解释器用来产生一个值 ——JS权威指南

#### 原始表达式:

    1.常量，直接量 3.14  'test'
    2.关键字 null this true
    3.变量    i  j  k

    原始表达式和原始表达式可以通过运算符来构成——符合表达式
    10*20 

#### 数组、对象的初始化表达式

    [1,2]        等价于     new Array(1,2)
    [1,,,4] 		     	[1,undefined , undefined ,4]
    {x:1 , y:2}		var o = new Object(); o.x = 1 ; o.y = 2 ;

#### 函数表达式

    var fr = function(){}
    (function(){console.log('hello world');})();

#### 属性访问表达式

    var o = {x:1}
    o.x   o['x']

#### 对象创建表达式

    new Func(1,2) ;
    new Object ;

### 运算符operators

运算符:表达式之间做运算

#### **按操作数分
    一元运算符： +num
    二元         a + b 
    三元        c ? a : b


#### **按功能区分
    赋值   x+= 1  -=    =
    比较   a == b
    算数   a - b   + - * / %
    位     a | b
    逻辑   exp1 && exp2
    字符串  "a" + "b"
    特殊   delete obj.x


#### **逗号运算符

var val = (1,2,3)   //val = 3

从左到右依次计算表达式的值，最后取最右边的


#### **delete

    var obj = {x:1}
    obj.x;   //1
    delete obj.x ;
    obj.x    //undefined

IE9开始(ES5)

    var obj = {}
    Object.defineProperty(obj,'x',{
    	configurable : false,
    	value : 1
    });

    delete obj.x ; //false  configurable设置为true才能被delete
    obj.x ;        // 1


#### ** 运算符 in

    window.x = 1 ;
    'x' in window ; //true

#### ** 运算符 instanceof  typeof

    {} instanceof Object ;     //true instanceof 判断对象的类型,基于原型链
    typeof 100 === 'number' ;  //true 判断原始类型或函数对象

#### ** 运算符 new

    function Foo(){};
    Foo.prototype.x = 1;
    var obj = new Foo();
        obj.x ;                          //1
        obj.hasOwnProperty('x') ;        //false
        obj._proto_.hasOwnProperty('x'); //true  chrome里可以查到


#### ** 运算符this

    this ; //window
    var obj = {
    	func:function(){
    		return this ;
    	}
    }
    obj.func(); //obj
 
如果用obj.func().apply()可以指定this的值

#### ** 运算符void 一元运算符

    void 0  //undefined
    void(0) //undefined

***** 运算符优先级

## block语句 、var语句

    block 块语句常用语组合0~多个语句，块语句用一堆花括号定义
    语法：
    {
    	语句1;
    	语句2;
    	...
    	语句n
    }


### try catch语句

有三种形式: 1, try  catch  2.try finally 3. try catch finally

嵌套时

第一:
    try{
		  try{
			  throw new Error("oops");
	  	}
		finally{
			console.log("finally");
		  }
    }
    catch(ex){
		  console.error("outer" , ex.message);
    }

    // "finally"
    // "outer" "oops"

第二：

    try{
		  try{
			  throw new Error("oops");
      }
		  catch(ex){
			  console.error("inner" , ex.message);
		   }
		  finally{
		  	console.log("finally");
      }
    }
    catch(ex){
		  console.error("outer" , ex.message);
    }

    // "innner" "oops"
    // "finally"
    异常已经在内部catch过了 所以不会抛到外面再处理

第三

    try{
		  try{
			  throw new Error("oops");
		  }
		  catch(ex){
			  console.error("inner" , ex.message);
			  throw ex ; //内部还在抛出个异常
		  }
		  finally{
			  console.log("finally");
		  }
    }
    catch(ex){
		  console.error("outer" , ex.message);
    }

    // "innner" "oops"
    // "finally"
    // "outer" "oops"



#### ------------ 7.函数 switch 循环 ---------------

用function语句定义的函数对象，我们叫他函数声明:

		fd();                   // true
		function fd(){
		//do sth
		return true;
	}

函数表达式:

		fe();                   // TypeError
		var fe = function(){
			//do sth
		}

函数声明会被预先处理，或者函数前置。
函数表达式就不能再声明前调用 会TypeError


**for in 语句 **

		var p ;					// 1.顺序不确定
		var obj = {x:1 , y:2}		        // 2.enumerable为false时不会出现
		for( p in obj ){			// 3.for in 对象属性受原型链影响
		}


#### ------------ 8.严格模式 ---------------

严格模式是一种特殊的执行模式，它修复了部分语言上的不足，提供更强的错误检查，并增强安全性

```
function func(){
	'use strict';
}
```

*不允许用with 会有个SyntaxError 语法错误

*不允许未声明的变量被赋值 , 会有个ReferenceError
 这在一般模式下为声明变量是全局变量

*arguments变为参数的静态副本


**一般模式**
```
!function(a){
	arguments[0] = 100;
	console.log(a);    //100
}(1); 

```
要注意的是如果不把1传进去那console.log(a) 打印出的undefined
一般模式下形参与arguments还是有绑定关系的，严格则没有

**严格模式**

```
!function(a){
	'use strict';
	arguments[0] = 100;
	console.log(a);    //1
}(1);


```
严格模式下 不论参数传还是不传，对arguments都不会有影响

但如果参数传的是一个对象的话
```
!function(a){
	'use strict';
	arguments[0].x = 100;
	console.log(a.x);     //100
}({x:1});

```

delete参数，函数名报错

```
不允许用with
所有变量必须声明，赋值给未声明的变量会报错，而不是隐士创建全局变量
eval中的代码不能创建eval所在作用域下的变量、函数。而是为eval单独创建一个作用域，并在eval返回时丢弃
函数中的特殊对象arguments是静态副本，而不像非严格（一般）模式下那样，修改arguments或修改参数变量会相互影响
删除configurable = false的属性时报错，而不是忽略
禁止八进制字面量，比如010（八进制的8）
eval , arguments变为关键字，不可作为变量名、函数名等
一般函数调用时（不是对象的方法调用，也不实用apply/call/bind等修改this)this指向null,而不是全局对象
若使用apply/call ,当传入null或undefined时，this将指向null或undefined，而不是全局对象
试图修改不可写属性（writable = false),在不可拓展的对象上添加属性时报TypeError,而不是忽略
arguments.caller  arguments.callee被禁用

严格模式是一种特殊的运行模式，修复了部分语言上的不足，比如禁用with
提供更强的错误检查，比如重复再字面量中写重复的属性名，或者尝试delete一些不可配置的属性，或者给没声明的变量赋值而创建了全局变量
安全性上 禁用 arguments.caller  arguments.callee
严格模式是向上兼容的。


```























