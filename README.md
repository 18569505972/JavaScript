# 基础知识笔记  
## viewport  
\<meta name='viewport' content='width=device-width,user-scable=no,initial-scale=1'\>  
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
function fn1(){  
	console.log(1+this.name)    
}  
function fn2(){  
	this.name='zhao';   
	console.log(2+this.name)   
}  
fn1.call(fn2) //1zhao   
//fn1.call=function(){this}，call方法中this指向fn1；fn1内部this指向fn2  
fn1.call.call(fn2) //2zhao   
//fn1.call方法中this指向fn2，所以打印2zhao  
### 区别：  
call与apply传参不同：fn1.call(fn2,'name','age');fn1.apply(fn2,['name','age'])  
call、apply与bind的区别：fn1.call(fn2,'name','age')立即执行；var binFn=fn1.bind(fn2,'name','age');binFn()返回一个值  
## sort排序  
arr.sort()//默认按ASCLL从小到大排序。（数字不准确，得用排序函数）  
arr.sort(function(a,b){   
	return a-b;//从小到大  
	return b-a;//从大到小  
})  
## 求数组最大最小值  
### 排序取值法：  
var arr = [8, 3, 4, 8, 9, 6, 2, 7];  
ary.sort(function (a, b) {  
    return a - b;  
});  
var min = arr[0];  
var max = arr[arr.length - 1];  
### 假设法  
var arr = [8, 3, 4, 8, 9, 6, 2, 7];    
var min=arr[0],max=arr[0];  
for(var i=0;i<arr.length;i++){  
	var cur=arr[i];  
	cur<min?min=cur:null;  
	cur>max?max=cur:null;  
}  
### Math中的max和min  
var arr = [8, 3, 4, 8, 9, 6, 2, 7];    
Math.min.apply(null,arr)  
Math.max.apply(null,arr)  




