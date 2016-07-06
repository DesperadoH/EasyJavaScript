##  4-1 对象

对象中包含一系列属性，这些属性是**无序的**，每一个属性都有一个**字符串**key和对应的value

```
var obj = {}
obj[1] = 1;
obj['1'] = 2;
obj;              // Object{1:2}

obj[{}] = true ;
obj[{x:1}] = true ;
obj ;             // Object{1:2,[object Object]:true}


```
内部会toString一下转化成字符串

### ----------------4.1对象的构造-------------------

```

var obj = {}
    obj.y = 2 ;
    obj.x = 1 ;


```
可以动态添加属性，并且每个属性都有：

writable  enumerable  configurable  value  get/set 等属性和方法

对象还有个原型 [[proto]]

```
function Foo(){}
Foo.prototype.z = 3 ;

var obj = new Foo();
    obj.z ;         //3

```

除此之外，对象中还会有class标签 [[class]] 来表示是哪一个种类的
还有[[extensible]]标签还表示是否允许继续增加新的属性

### -------------4.2创建对象-------------

**1. 对象字面量 {x:1 , y:2}**

**2. new构造器创建 原型链**


function Foo(){}
默认会带一个 prototype属性，是一个**对象**属性

Foo.prototype.z = 3;

```
var obj = new Foo();
obj.y = 2 ;
obj.x = 1 ;
obj.z     ; //从原型链上查找
```

obj的原型会指向 构造器的peototype属性，也就是Foo.prototype属性

```
obj.x ; //1
obj.y ; //2 
obj.z ; //3
typeof obj.toString ;      //'function'
'z' in obj ;               //true 
obj.hasOwnProperty('z') ;  //false

obj.prototype原型指向 Foo.prototype原型指向Object.prototype也是有原型的 指向null

```

**3.Object.create**

obj ---> {x:1} ---->Object.prototype --->null


```

var obj = Object.create({x:1});
obj.x ;  				    //1
typeof obj.toString ;       //'function'
obj.hasOwnProperty('x');    //false

var obj = Object.create(null);
obj.toString 				//undefined

```

### -------- 4.3属性操作 -----------

读写对象属性  属性异常  删除属性 检测属性 枚举属性

**读写**

```
var obj = {x1:1 , x2:2 };
var i = 1 , n = 2;
    for(; i <= n ; i++ ){
	     console.log(obj['x' + i]);
    }
//输出 1 , 2
var p ;
    for( p in obj ){
       console.log(obj[p]);
  }

```

**读写异常**
```
var obj = { x:1 };
    obj.y ;        //undefined
var yz = obj.y.z ; //TypeError:Cannot read property 'z' of undefined
    obj.y.z = 2 ;  //TypeError: Cannot set property 'z' of undefined

var yz;
if(obj.y){
	yz = obj.y.z;      var yz = obj && obj.y && obj.y.z ;
}

```

**属性删除**

```
var person = {age:28 , title:'fe'} ;
	   delete person.age ;               //true
     delete person['title'] ;          //true

person.age ;                           //undefined
     delete person.age ;        		   //true

     delete Object.prototype ;         //false 不允许删除

var descriptor = Object.getOwnPropertyDescriptor(Object , 'prototype')
    descriptor.configurable ;          //false -->决定了delete Object.prototype是不行的

```


```
var globalVal = 1 ;
delete globalVal ; 		 //false

(function(){
	var localVal = 1;
	return delete localVal;
}());  					 //false

function fd(){
	delete fd;           //false
}
(function(){
	function fd(){};
	return delete fd ;
}());                    //false


```
**值得注意的是**

隐式创建全局变量 但不推荐

```
ohNo = 1 ;
window.ohNo ;  //1
delete ohNo ;  //true

//eval定义的变量也可以被delete

```

#### 属性检测

```
var cat = new Object;
    cat.lengs = 4;
    cat.name = 'Kitty';

'legs' in cat ;     //true
'abc'  in cat ;     //false
'toString' in cat ; //true , inherited property!!!

cat.hasOwnProperty('legs');     //true
cat.hasOwnProperty('toString'); //false

cat.propertyIsEnumerable('leg');      //true  可枚举
cat.propertyIsEnumerable('toString'); //false 不可枚举

```

自定义个属性让他的枚举属性为false

```
Object.defineProperty(cat , 'price' , {enumerable:false , value :1000});
cat.propertyIsEnumerable('price') ; //false
cat.hasOwnProperty('price')  ;      //true


```

#### *****属性枚举*****

```
var o = {x:1 , y:2 , z:3 }
'toString' in o ;         			 //true
o.propertyIsEnumerable('toString');  //false
var key;
for( key in o ){
	console.log( key ) ; //x,y,z
}


var obj = Object.create(o);
	obj.a = 4;
var key ;
for( key in obj ){
	console.log(key) ; // a x y z
}


var obj = Object.create(o);
	obj.a = 4;
var key ;
for( key in obj ){
	if( obj.hasOwnProperty(key) ){
		console.log(key) ; // a 
	}	
}


```

#### **get/set方法**

```

var man = {
	name : 'Bosn',
	weibo: '@Bosn',
	get age(){
		return new Date().getFullYear() - 1988;
	},
	set age(val){
		console.log("Age can\'t be set to " + val);
	}
}
//console.log 自动调用get方法
console.log(man.age) ; // 27

//赋值的话，自动调用set方法
man.age = 100 ;        // Age can't be set to 100

console.log(man.age) ; // 27


```

```
var man = {
	weibo:'@Bosn',
	$age : null ,
	get age(){
		if(this.$age == undefined){
			return new Date().getFullYear() - 1988;
		}else{
			return this.$age;
		}

	},
	set age(val){
		val = +val ;
		if(!isNaN(val) && val > 0 && val < 150 ){
			this.$age = +val ;
		}else{
			throw new Error('Incorrect val =' + val );
		}
	}
}
console.log(man.age) ;  //27
man.age = 100 ;
console.log(man.age) ;  //100
man.age = 'abc' ;       //error:Incorrect val = NaN ;



```

#### *****get/set与原型链*****


```
function Foo(){}
Object.defineProperty(Foo.prototype , 'z' , {get : function(){return 1;}});
var obj = new Foo();

	obj.z ; 	//1
	obj.z = 10 ;  
	obj.z ; 	// still 1

```

解决：


```

Object.defineProperty(obj , 'z' , {value :100 ,configurable:true});
	   obj.z ; //100
delete obj.z ;
	   obj.z ; //back to 1


```

----------------------------------

```

ar o = {};
Object.defineProperty(o , 'x' ,{value : 1 });
// 默认 writable = false , configurable = false
var obj = Object.create(o);
	obj.x;     //1
	obj.x = 200;
	obj.x;     //still 1 , can't change it

Object.defineProperty(obj , 'x' , {value:100 , writable:true ,configurable:true});
	obj.x; //100
	obj.x = 500 ;
	obj.x; //500


```

### ---------4.5属性标签-----------

属性级的权限设置

```

Object.getOwnPropertyDescriptor({pro:true} , 'pro');  //Object{value:true , writable:true ,enumerable:true ,configurable:true}
Obhect.getOwnPropertyDescriptor({pro:true} , 'a' );   //undefinde

var person = {};
Object.defineProperty(person , 'name' , {
	configurable : false,
	writable: false ,
	enumerable : true ,
	value : "Bosn Ma"
});


person.name ; //Bosn Ma 
person.name = 100 ; 
person.name ; //Bosn Ma 
delete person.name ; //false

---------------------------------------

Object.defineProperty( person , 'type' , {
	configurable: true ,
	writable: true ,
	enumerable: false ,
	value: 'Object'
});

Object.keys(person);  // ["name"]

```

定义多个属性

```

Object.defineProperties( person , {
	title:{value: 'fe', enumerable : true},
	crop:{ value: 'BABA', enumerable : true},
	salary:{ value: 50000 , enumerable:true, writable: true},
	luck:{
		get:function(){
			return Math.random() > 0.5? 'good' : 'bad';
			}
	}
	promote:{
		set:function(level){
			this.salary *= 1 + level*0.1;
		}
	}

});

Object.getOwnPropertyDescriptor(person , 'salary');
//Object{value:5000 , enumerable:true, writable:true, configurable:false}
Object.getOwnPropertyDescriptor(person , 'corp');
//Object{value:'BABA' , enumerable:true, writable:false, configurable:false}

person.salary; //50000
person.promote = 2; 
person.salary; //60000


```

### ---------4.6 对象标签 对象序列化-----------

对象级别的标签

[[proto]] 原型 , 原型链是通过对象.proto去实现的

[[class]]  

[[extensible]]

class标签 ，表示对象是什么类型
因为没有直接的方式去查看他，所以用toString方法

```
var toString = Object.prototype.toString;
function getType(o) { return toString.call(o).slice(8,-1) };
//从第八个字符开始一直到最后，然后去除最后一个

```

```
toString.call(null) ;   // "[object Null]"
getType(null) ;         // "Null"
//slice就是把 Null 前后的东西去除，方便看

```

```

getType(undefined) ;        // "Undefined";
getType(1) ;                // "Number"
getType(new Number(1)) ;    // "Number"
typeof new Number(1) ;      // "object"
getType(true) ;             // "Boolean"
getType(new Boolean(true)); // "Boolean"

```

extensible标签
对象上的属性是否可扩展

```
var obj = {x:1 , y:2};
Object.isExtensible(obj) ; //true
Object.preventExtensions(obj);
Object.isExtensible(obj) ; //false

obj.z = 1 ;
obj.z ; //undefined , add new property failed
Object.getOwnPropertyDescriptor(obj , 'x');
// Object {value:1,writable:true ,enumerable:true,configurable:true}

Object.seal(obj) ;
Object.getOwnPropertyDescriptor(obj , 'x') ;
// Object {value:1,writable:true ,enumerable:true,configurable:false}
Object.isSealed(obj) ; //true

Object.freeze(obj) ;
Object.getOwnPropertyDescriptor(obj , 'x') ;
// Object {value:1,writable:false ,enumerable:true,configurable:false}
Object.isFrozen(obj) ; //true



```

**对象序列化，吧数据传给后端**

```
var obj = {x:1 , y:true , z:[1,2,3] ,nullVal:null};

JSON.stringify(obj) ; //"{"x":1 , "y":true , "z":[1,2,3] ,"nullVal":null}"

```


```
obj = { "val" : undefined , "a" : NaN , "b" : Infinity, "c" : new Date()};
JSON.stringify(obj);
"{ "a" : null, "b" : null, "c" : new Date()}"
//序列化是有坑的，如果值为undefined 就不会出现在结果中
//如果值是 NaN , Infinity ,则结果里就会变为null

obj.JSON.parse('{"x" : 1}');
obj.x; //1

```

**自定义序列化过程**


```
var obj = {
	x:1 ,
	y:2 ,
	o: {
		o1:1,
		o2:2,
		toJSON:function(){
			return this.o1 + this.o2 ;
			//this 指向o
		}
	}
};

JSON.stringify(obj) ; //"{ "x" : 1 , "y" : 2 , "o" : 3 }"


```

**其他对象方法 toString , valueOf**

```

var obj = { x:1 , y:2 };
obj.toString() ; //"[object Object]"
obj.toString = function(){
	return this.x + this.y;
};
//可以自己去定义自己对象上的toString



"Result" + obj ; // "Result3" , by toString
//理解成字符串拼接，然后obj就直接自己调用toString

+obj ; //3,from toString


obj.valueOf = function(){
	return this.x + this.y + 100 ;
};
//valueOf——尝试把对象转化为基本类型的时候会调用的函数

+obj; //103 ,from value Of
//一元加号转化为数字

"Result" + obj ; //still "Result103"

//一元+号运算符，二元字符串拼接，会尝试把对象转化为基本类型，先调用
valueOf, 如果valueOf 返回基本类型 那就以valueOf的值作为结果，
反之valueOf不存在或者返回的是对象，就会去找toString,如果toString和valueOf都没有，或者都返回对象，那就会报错.js解析器会自动调用这些方法


```





























