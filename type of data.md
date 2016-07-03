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





















