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
##字符串的扩展
unicode优化，es6之前js只能识别\u0000——\uFFFF之间字符；es6可以通过添加花括号识别。
```
// es6之前
\uD842\uDFB7
// es6
\u{20BB7}
// "𠮷"
```
codePointAt:正确返回32位的UTF-16字符的十进制码点。
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
```
fromCodePoint:正确解析32位的UTF-16字符的十进制码点。
```
String.fromCodePoint(0x20BB7)
// "𠮷"
```
字符串具有iterator接口，可被for...of遍历。
```
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"
```




