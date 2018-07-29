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
### 比较符==转换规则  
object比较引用地址（存储地址）。[]==false,true，array的toString方法，会用逗号链接每个元素，空数组转换为''，所以返回true。   
### 类数组对象arguments转数组  
```javascript
[].slice.call(arguments)   
```
### for循环异步问题  
```javascript
for (var i = 0; i < 5; i++) {  
  setTimeout(function() {  
    console.log(i);  
  }, 1000 * i);  
}  
```
####es6使用let声明循环参数：  
```javascript
for (let i = 0; i < 5; i++) {  
  setTimeout(function() {  
    console.log(i);  
  }, 1000 * i);  
}  
```
####闭包：
```javascript
for (let i = 0; i < 5; i++) {  
  (function(){setTimeout(function() {  
    console.log(i);  
  }, 1000 * i)})(i);  
}  
```
### js性能优化  
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






