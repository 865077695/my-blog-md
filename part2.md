---
title: 详解JavaScript存储_sessionStorage-localStorage
date: 2016-10-24 14:31:00
tags: Skill
---
localStorage 与 sessionStorage用法相同，区别在于 sessionStorage 在关闭页面后即被清空，而 localStorage 则会一直保存。
 localStorage 是 HTML5 本地存储的 API，使用键值对的方式进行存取数据，存取的数据只能是字符串。不同浏览器对该 API 支持情况有所差异，如使用方法、最大存储空间等。
* 存储内容： 只要是能序列化成字符串的内容都可以存储，利用JSON.stringify()；
* 存储大小： 5m
* 兼容性：    ie8+
* 主要应用：常用于ajax请求缓存
* 缺陷：
1. localstorage不被爬虫识别，所以不能用它完全取代url传參
2. 不能跨域共享，所以不要用以存储业务关键信息，更加不要存储安全信息，（cookie可以通过window.name解决，但是localstorage不能）

### 常见应用：
1. 判断localstorage已经存储满了
```js
try {
    localStorage.setItem(key, JSON.stringify({data: value, time: curTime}));
} catch (e) {
    //如果存储满了,就全部删掉
    localStorage.clear();//全部删除
    localStorage.setItem(key, JSON.stringify({data: value, time: curTime}));
}
```
存储满后会抛出异常，只要捕获异常，再删除全部数据，即可。
JSON.stringify(localStorage).length   可以查看当前使用了的大小，用5M减一下可以得出粗略的剩余大小（但是很不精确）
2. 判断localstorage已过期  （由于localstorage没有cookie的过期控制，需要自己控制）
```js
function set(key, value) {
    var curTime = new Date().getTime();
    try {
        localStorage.setItem(key, JSON.stringify({data: value, time: curTime}));
    } catch (e) {
        //如果存储满了,就全部删掉
        localStorage.clear();//全部删除
        localStorage.setItem(key, JSON.stringify({data: value, time: curTime}));
    }
}
function get(key, exp) {
    var data = localStorage.getItem(key);
    var dataObj = JSON.parse(data);
    if (new Date().getTime() - dataObj.time > exp) {
        localStorage.removeItem(key);//过期就清除key的值
        console.log('信息已过期');
    } else {
        return JSON.stringify(dataObj.data);
    }
}
```
3. 判断浏览器是否支持localstorage
```js
if (window.localStorage) {
    console.log('This browser supports localStorage');
} else {
    console.log('This browser does NOT support localStorage');
}
```
4. 常见api
```js
localStorage.setItem("b", str); //设置b的值
var b = localStorage.getItem("b");  //获取b
localStorage.clear();//全部删除
localStorage.removeItem(key);//清除key的值
```
### 总结：

H5的两种存储技术的最大区别就是生命周期。localStorage是本地存储，存储期限不限；sessionStorage会话存储，页面关闭数据就会丢失。

使用方法：

* localStorage.setItem（“key”，“value”）//存储
* localStorage.getItem（key）//按key进行取值
* localStorage.valueOf( )//获取全部值
* localStorage.removeItem(key)//删除单个值
* localStorage.clear()//删除全部数据
* localStorage.length//获得数据的数量
* localStorage.key(N)//获得第N个数据的key值
* 注：localStorage和sessionStorage用法相同
