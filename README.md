# 基础知识笔记  
## viewport  
```javascript
<meta name='viewport' content='width=device-width,user-scable=no,initial-scale=1'>  
```
width设置布局视口的宽度，为一个正整数  
initial-scale设置页面的初始缩放程度 和布局视口的宽  
minimum-scale允许用户的最小缩放程度，为一个数字，可以带小数  
maximum-scale允许用户的最大缩放值，为一个数字，可以带小数  
user-scalable是否允许用户进行缩放，值为”no”或”yes”, no 代表不允许，yes代表允许  
## cookies，sessionStorage和localStorage的区别、用法  
### 区别：   
1： 存储大小  
cookie数据大小不能超过4k。sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。  
2、有效时间  
localstorage永久储存，浏览器关闭不清除，只能手动清除。sessionstorage临时存储，关闭当前浏览器窗口删除。cookie设置有效时间内一直有效。  
3、数据与服务器间的交互  
cookie的数据会自动的传递到服务器，服务器端也可以写cookie到客户端。sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。  
4、不同浏览器无法共享localStorage和sessionStorage，不同页面同域（协议、端口、域名一致）可共享localStorage，不可共享sessionStorage（除了标签页同域嵌入的iframe）  
### 使用：  
#### cookie：
设置cookie：自定义参数、过期时间（expirse）、页面路径  
document.cookie="name=zhao;age=15;expirse="+(new Date().getTime()+1*24*60*60*1000)+";path=/zhao"  
#### sessionstorage、localStorage有相同用法：   
存值：sessionStorage.setItem('key','value')  
取值：sessionStorage.getItem('key')  
删除：sessionStorage.removeItem('key')//删除指定值sessionStorage.clear()//删除所有值  
获取键名：sessionStorage.key(i)//i为枚举序号  
## call、apply、bind用法区别  
### 用法：  
```javascript
function fn1(){   
　　console.log(1+this.name)    
}  
function fn2(){  
　　this.name='zhao';   
　　console.log(2+this.name)   
}  
fn1.call(fn2) //1fn2   
fn1.call(fn2()) //1zhao  
//fn1.call=function(){this}，call方法中this指向fn1；fn1内部this指向fn2   
```
### 区别：  
call与apply传参不同：fn1.call(fn2,'name','age');fn1.apply(fn2,['name','age'])  
call、apply与bind的区别：fn1.call(fn2,'name','age')立即执行；var binFn=fn1.bind(fn2,'name','age');binFn()返回一个值  
## sort排序  
arr.sort()//默认按ASCLL从小到大排序。（数字不准确，得用排序函数）  
```javascript
arr.sort(function(a,b){   
　　	return a-b;//从小到大  
　　	return b-a;//从大到小  
}) 
``` 
## 求数组最大最小值  
### 排序取值法：  
```javascript
var arr = [8, 3, 4, 8, 9, 6, 2, 7];  
ary.sort(function (a, b) {  
　　 return a - b;  
});  
var min = arr[0];  
var max = arr[arr.length - 1];  
```
### 假设法  
```javascript
var arr = [8, 3, 4, 8, 9, 6, 2, 7];    
var min=arr[0],max=arr[0];  
for(var i=0;i<arr.length;i++){  
　　	var cur=arr[i];  
　　	cur<min?min=cur:null;  
　　	cur>max?max=cur:null;  
}  
```
### Math中的max和min  
```javascript
var arr = [8, 3, 4, 8, 9, 6, 2, 7];    
Math.min.apply(null,arr)  
Math.max.apply(null,arr)  
```
## 比较符==转换规则  
object比较引用地址（存储地址）。[]==false,true，array的toString方法，会用逗号链接每个元素，空数组转换为''，所以返回true。   
## 类数组对象arguments转数组  
```javascript
[].slice.call(arguments)   
```
## for循环异步问题  
```javascript
for (var i = 0; i < 5; i++) {  
  setTimeout(function() {  
    console.log(i);  
  }, 1000 * i);  
}  
```
###es6使用let声明循环参数：  
```javascript
for (let i = 0; i < 5; i++) {  
  setTimeout(function() {  
    console.log(i);  
  }, 1000 * i);  
}  
```
###闭包：
```javascript
for (let i = 0; i < 5; i++) {  
  (function(){setTimeout(function() {  
    console.log(i);  
  }, 1000 * i)})(i);  
}  
```
## js性能优化  
#### 函数防抖:  
概念：函数节流是通过一个定时器，阻断连续重复的函数调用，从而一定程度上优化性能。  
用途：主要用于用户界面调用的函数，如：resize、mousemove、keyup事件的监听函数。   
这类监听函数的主要特征：   
1、短时间内连续多次重复触发；   
2、大量的DOM操作。  
意义：在用户察觉范围外，降低函数调用的频率，从而提升性能。目的是只有在执行函数的请求停止了一段时间之后才执行。  
```javascipt  
<input id="search" type="text">  
var search=document.getElementById(search)  
function searchText(text){  
	console.log('search'+text)  
}  
search.addEventListener('keyup',function(event){  
	debounce(searchtext,null,500,this.value)  
})  
function debounce(fn,fn2,delay,text){  
	clearTimeout(fn.timeId)  
	fn.timeId=setTimeout(function(){  
		fn.call(context,text);  
	},delay)  
}  
```
#### 函数节流：  
```javascript
var canRun=true;  
document.getElementById('throttle').onscroll=function(){  
	if(!canRun){  
		return;  
	}  
	canRun=false;  
	setTimeout(function(){  
		canRun=true;
		console.log('throttle');  
	},500)  
} 
``` 
## 垃圾回收机制  
### 1、标记清除  
进入执行环境，给变量标记为进入环境，变量离开执行环境，标记为离开环境。垃圾收集器，先给内存中所有变量打上标记，然后去掉环境中的变量以及被环境中变量引用的标记，之后再被标记上的变量就是要清除的变量。
### 2、引用计数  
声明一个变量，当他被引用赋值时，则记录一次引用次数，包含这个值的引用变量又取得了另外的值，则这个值引用次数减1。引用次数为0，则释放值所占用的内存。  
## 浏览器缓存机制  
### 类型：
强缓存：缓存未过期，使用浏览器缓存 200  
协商缓存：缓存未过期，向服务器发起请求询问是否用缓存，头中携带last-modefied、etag（标识对象是否发生改变） 304  
### 相同和不同点  
##### 相同：
都从浏览器缓存取数据  
##### 不同：
强缓存不与服务器交互，协商缓存与服务器交互  
### 缓存有效期  
#### expirse 
指定过期时间，存在服务器与客户端时间不一致，会存在缓存偏差问题。  
#### cache-control
max-Age:最大缓存时间  
no-cache：表明资源不进行缓存。但是设置了no-cache之后并不代表浏览器不缓存，而是在缓存前要向服务器确认资源是否被更改。因此有的时候只设置no-cache防止缓存还是不够保险，还可以加上private指令，将过期时间设为过去的时间。  
no-store：绝对禁止缓存。  
## 算法  
1、冒泡排序  
```javascript
function bubbleSort(arr) {
　　var len = arr.length;
　　for (var i = 0; i < len; i++) {
　　　　for (var j = 0; j < len - 1 - i; j++) {
　　　　　　if (arr[j] > arr[j+1]) { //相邻元素两两对比
　　　　　　　　var temp = arr[j+1]; //元素交换
　　　　　　　　arr[j+1] = arr[j];
　　　　　　　　arr[j] = temp;
　　　　　　}
　　　　}
　　}
　　return arr;
}
```
2、选择排序  
```javascript 
function selectionSort(arr) {
　　var len = arr.length;
　　var minIndex, temp;
　　console.time('选择排序耗时');
　　for (var i = 0; i < len - 1; i++) {
　　　　minIndex = i;
　　　　for (var j = i + 1; j < len; j++) {
　　　　　　if (arr[j] < arr[minIndex]) { //寻找最小的数
　　　　　　　　minIndex = j; //将最小数的索引保存
　　　　　　}
　　　　}
　　　　temp = arr[i];
　　　　arr[i] = arr[minIndex];
　　　　arr[minIndex] = temp;
　　}
　　console.timeEnd('选择排序耗时');
　　return arr;
}
```
3、快速排序  
```javascript
function quickSort(a) {
  if (a.length <= 1) return a;

  var last = a.pop()
    , left = []
    , right = [];

  a.forEach(function(item) {
    if (item <= last)
      left.push(item);
    else
      right.push(item);
  });

  var _left = quickSort(left)
    , _right = quickSort(right);

  return _left.concat(last, _right);
}
```
4、数组去重    
```javascript
let unique =  function(arr){
   let hash={};
   let data=[];
    for (let i=0;i<arr.length;i++){
         if (!hash[arr[i]])  {
               hash[arr[i]]=true;
                data.push(arr[i]);
      }      
   }
   return data

}
```
5、统计字符串出现最多的字母  
```javascript
function findMax(str){     
	if  (str.length ==1){
		return str;
	}
	let charObj = {};
	for (let i=0;i<str.length;i++) {
		if(!charObj[str.charAt(i)]){
		   charObj[str.charAt(i)]=1;
		} else{
		     charObj[str.charAt(i)]+=1;
		}

	}
	let maxChar='',
		maxValue=1;
	for (var k in charObj){
	  if (charObj[k]>=maxValue){
	     maxChar=k;
	     maxValue = charObj[k];
		}
	}
	return maxChar;
}
```
6、变量互换  
```javascript
function swap（a,b）{
    b=b-a;
    a=a+b;
    b=a-b;
    return [a,b];
}
```
7、随机生成指定长度的字符串  
```javascript
function random(n){
   let str='abcdefghijkmnopqrstuvwxyz9876543210';
   let tmp='',
       i=0,
       l=str.length;
  for(i=0;i<n;i++){
    tmp +=str.charAt(Math.floor(Math.random()*l))
    }
    return tmp;
 }
 ```
 8、找出下列正整数组的最大差值  
 ```javascript
 function getMaxPro(arr){
    var minPrice=arr[0];
    var maxProfit=0;
    for (var i=0;i<arr.length;i++){
       var currentPrice=arr[i];
       minPrice=Math.min(minPrice,currentPrice);
       var potentialProfit =currenrPrice-minPrice;
       maxProfit=Math.max(maxProfit,potentialProfit);
    }
    return maxProfit;  
}
```
9、斐波那契数列   
斐波那契数列：1、1、2、3、5、8、13、21、34、……    
```javascript
function fn(n){
	if(n==1|n==2){
	return 1;}
	//因为斐波那契数列格式为：1、1、2、3、5、8、13、21、34、......,n=1和n=2的时候都是输出1
	return fn(n-1)+fn(n-2);
	//不断调用自身函数，n-1是穿进去的参数的前一次，就是最后n的前一个数字。所以n-2是最后传入参数的前两个数字。
}
```





