# es6基础知识  
## 解构赋值 
解构赋值右边不是可遍历的结构Iterator，则会报错。  
```
let [a,b,...c]=['a']
/*
  a: a 
  b: undefined
  c: []
*/

let [x=2,y=1]=[1]
/*
  x:1
  y:1
 */
// ES6通过内部严格相等符号===undefined来判断一个位置是否有值。
let [x=2,y=3]=[null,undefined]
/*
  x:null
  y:3
 */
// 默认值可以一个表达式
function f() {
  return 666;
}
let [a=f()]=[]
/*
  a:666
 */
```
数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。  
```
var {foo,bar}={foo:1,bar:2};
/*
  foo:1
  bar:2 
 */
// 对象解构赋值，先找到对应属性，再给赋值
var {foo:a,bar:b}={foo:1,bar:2}
/*
  a:1
  b:2 
 */
 // 嵌套赋值
 let obj = {};
 let arr = [];
 ({foo:obj.foo,bar:arr[0]}={foo:1,bar:2})
 /*
   obj:{foo:1}
   arr:[2] 
  */
```
基本值类型解构，会先将其转换为对象，undefined和null无法结构会报错。
```
let [a,b,c]='hello'
let {length:len}='hello'
/*
  a:h
  b:e
  c:l 
  len:5
 */
```
## 字符串的扩展
unicode优化，es6之前js只能识别\u0000——\uFFFF之间字符；es6可以通过添加花括号识别。
```
// es6之前
\uD842\uDFB7
// es6
\u{20BB7}
// "𠮷"
```
codePointAt:正确返回32位的UTF-16字符的十进制码点。  
字符串具有iterator接口，可被for...of遍历。  
fromCodePoint:正确解析32位的UTF-16字符的十进制码点。  
at：替代之前的charAt返回给定位置字符，可以识别码点大于0xFFFF的字符。  
```
// 可以通过for...of正确遍历32位UTF-16字符
var s = '𠮷a';
s.codePointAt(0).toString(16) // "20bb7"
s.codePointAt(2).toString(16) // "61"
for(let ch of s) {
  console.log(ch.codePointAt().toString(16))
}
// 20bb7
// 61
// 检测字符为四字节还是两字节，四字节返回true
function is32Bit(c) {
  return c.codePointAt(0) > 0xFFFF;
}
String.fromCodePoint(0x20BB7)
// "𠮷"
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"
'𠮷'.charAt(0) // "\uD842"
'𠮷'.at(0) // "𠮷"
```
indludes(findStr,startIndex):返回布尔值，字符串是否包含某字符串。  
startsWith(findStr,startIndex):返回布尔值，字符串是否以某字符串开头。    
endsWith(findStr,endIndex):返回布尔值，字符串是否以某字符串结尾。  
repeat(n):返回新字符串，将某字符串重复n次。  
padStart(strLength,str):字符串不满足strLength长度，重复使用str从字符串头部补齐到strLength。   
padEnd(strLength,str):字符串不满足strLength长度，重复使用str从字符串尾部补齐到strLength。   
```
'adcd'.includes('c',2) // true
'abcd'.startsWith('c',3)  // false
'abcd'.endsWith('c',2)  // false
'abcd'.endsWith('b',2)  // true
'a'.repeat(3)   // aaa
'a'.padStart(5, 'abc') // abcaa
'a'.padEnd(5, 'abc') // aabca
```
模板字符串:\`${}\`    
String.raw():替换模板字符串变量，返回源字符串的转义字符串 。   
```
// 函数调用模板字符串
function temStr(...tem) {
  for(let i =0;i<tem.length;i++) {
    console.log(tem[i])
  }
}
let str = 'def';
let str2 = 'ij';
temStr`abc${str}gh${str2}`
/*
  ['abc','gh','']
  'def'
  'ij'
*/
//String.row()
let a = 'bcd';
String.raw`a\n${a}e` // 实际返回a\\nbcde，浏览器显示a\nbcde
String.raw`a\\n${a}e` // 实际返回a\\nbcde，浏览器显示a\\nbcde
```
## 数值扩展
Number.isFinite(num):判断数字num是否有限。  
Number.isNaN(num):判是否为NAN。  
Number.isInteger(num):判断是否为整数。（此方法判断int.0也为整数）。    
Number.isSafeInteger(num):判断整数num是否在js可识别数字区间内。  
指数运算符：2**3=8。  
## 数组扩展
Array.from(obj,mapLoop,this):可以将类数组（含有length属性）或者可遍历对象转换为数组。（es5写法：[].slice.call(obj)）  
target.find(funcion(value,index,arr)=>{value>0}):返回target中第一个匹配值，没有匹配值返回undefined。  
target.findIndex((value,index,arr)=>{value>0}):返回target中第一个匹配值下标，没有匹配值返回undefined。     
arr.includes(value，searchIndex):数组中是否包含value。 
arr.forEach((value,index,arr)=>{}):forEach不返回值，不改变原数组。   
arr.map((value,index,arr)=>{return value}):返回新数组，不能深复制。  
for...of:遍历可迭代对象。  
arr.filter((value,index,arr)=>{return value>0}):返回满足条件的元素组成的新数组。  
arr.every((value,index,arr)=>{return value>0}):判断是否每个元素符合条件，返回布尔值。  
arr.some((value,index,arr)=>{return value>0}):判断是否存在元素符合条件，返回布尔值。  
arr.reduce((preValue,currentValue,index,arr)=>{return preValue+currentValue}):数组值累加。  
keys，values，entriesf:返回遍历器对象，可被for...of遍历。  
## 函数扩展
es6可设置函数默认值。  
```
// 设置默认值的参数必须放于未设置默认值的后面
function foo(x,y=1){
  console.log(x,y)
}
foo(1)
/*
  1
  1
*/
``` 
扩展运算符，展开iterate对象。  
name属性:返回函数名（匿名函数返回空）。  
箭头函数:this取决于定义位置，它自身无this，不能使用yield、arguments、call、bind、apply以及new。 
尾调用、尾递归的内存优化，防止栈溢出。  









