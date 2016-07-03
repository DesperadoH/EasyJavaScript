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












































