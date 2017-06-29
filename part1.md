---
title: 详解JavaScript存储_Cookie
date: 2016-10-24 10:44:11
tags: Skill
---
## 详解JavaScript存储

JavaScript用于存储的方式多种多样，善于应用存储将大大提高网站性能，接下来几节将分别介绍cookie、localstorage、sessionstorage、离线缓存（application cache）的用法。

### cookie
 * 存储大小:4kb左右，以20个为上限
 * 清理机制:IE和opera会清理近期最少使用的cookie，Firefox会随机清理cookie
 * 主要应用:购物车，登陆状态
 * 缺陷:同域内http请求都会携带cookie，浪费带宽

**cookie是字符串而且还是一个特定格式的文本字符串 **
**格式：cookieName=cookieValue;expires=expiresDate;path=URLpath;domain=siteDomain//cookie名称，失效日期，储存URL,储存域值；** 

cookie常见携带参数

| 属性名        |属性名介绍   |
| ------------- |-------------| 
| name=value    | 键值对，可以设置要保存的 Key/Value，注意这里的 NAME 不能和其他属性项的名字一样        | 
| Expires/max-age | 过期时间，在设置的某个时间点后该 Cookie 就会失效，如 expires=Wednesday, 09-Nov-99 23:12:40 GMT | 
| Domain        | 生成该 Cookie 的域名，如 domain=”soulteary.com”        | 
| Path        | 该 Cookie 是在当前的哪个路径下生成的，如 path=/admin/        | 
| Secure        | 如果设置了这个属性，那么只会在 SSH 连接时才会回传该 Cookie        | 
| http        | http-only   true：cookie只能在服务器端读取和修改，比较安全        | 
#### cookie安全
如果Cookie具有httpOnly属性且不能通过客户端脚本访问，则为true；否则为false。默认为false

ie 6 ＋都支持属性 HttpOnly，该属性有助于缓解跨站点脚本威胁。

#### 常见应用：
使用原生js操作cookie
* 设置cookie我们一般都封装成一个函数：
```js
function addCookie(sName,sValue,day) { 
    var expireDate = new Date(); 
    expireDate.setDate(expireDate.getDate()+day);; 
    //设置失效时间 
    document.cookie = escape(sName) + '=' + escape(sValue) +';expires=' + expireDate.toGMTString();6 //escape()汉字转成unicode编码,toGMTString() 把日期对象转成字符串 
} 
```
* 读取cookie
```js
function getCookies() { 
    var showAllCookie = ''; 
    if(!document.cookie == ''){ 
        var arrCookie = document.cookie.split('; '); //用spilt('; ')切割所有cookie保存在数组arrCookie中 
        var arrLength = arrCookie.length; 
        for(var i=0; i<arrLength; i++) { 
            showAllCookie += 'c_name:' + unescape(arrCookie[i].split('=')[0]) + 'c_value:' + unescape(arrCookie[i].split('=')[1]) + '<br>' 9 } 
        return showAllCookie; 
    } 
} 
```
* cookie有有效期可自动删除，也可以通过设置其失效日期来立即删除 
```js
function removeCookie() { 
    if(document.cookie != '' && confirm('你想清理所有cookie吗？')) { 
        var arrCookie = document.cookie.split('; '); 
        var arrLength = arrCookie.length; 
        var expireDate = new Date(); 
        expireDate.setDate(expireDate.getDate()-1); 
        for(var i=0; i<arrLength; i++) { 
         var str = arrCookie[i].split('=')[0]; 
         document.cookie = str+ '=' + ';expires=' + expireDate.toGMTString(); 
        } 
    } 
} 
```

* 运用cookie:
用cookie做的一个简单的计时器：
```js
var cookieCount = {}; 

cookieCount.count = function () { 
    var count = parseInt(this.getCount('myCount')); 
    count++; 
    document.cookie = 'myCount=' + count + ''; 
    alert('第'+count+'访问'); 
} 

cookieCount.setCount= function () { 
    //首先得创建一个名为myCount的cookie 
    var expireDate = new Date(); 
    expireDate.setDate(expireDate.getDate()+1); 
    document.cookie = 'myCount=' + '0' +';expires=' + expireDate.toGMTString(); 
} 

cookieCount.getCount = function (countName) { 
    //获取名为计数cookie,为其加1 
    var arrCookie = document.cookie.split('; '); 
    var arrLength = arrCookie.length; 
    var ini = true; 
    for(var i=0; i<arrLength; i++) { 
        if(countName == arrCookie[i].split('=')[0]){ 
            return parseInt(arrCookie[i].split('=')[1]); 
            break;
        }else{
            ini = false; 
        } 
    } 
    if(ini == false)this.setCount(); 
    return 0; 
} 

cookieCount.count(); 
```
* cookie的路径
 cookie的路径：path=URL; 
 如果在域名的子目录创建的cookie，域名及其他同级目录或上级目录是访问不到这个cookie的，而通过设置路径的好处就是可以使上域名以及域名的子类目录都可以访问到，如下： 
```js
document.cookie='cookieName=cookieValue;expires=expireDate;path=/'
```
* cookie域 
设置域：domain=siteDomain 
这个主要用在同域的情况下共享一个cookie，例如 "www.taobao.com" 与 "ued.taobao.com" 两者是共享一个域名"taobao.com"，我们如果想让 "www.taobao.com" 下的cookie被 "ued.taobao.com" 访问，那么就需要把path属性设置为 "/"，并且设置 cookie 的domain如下
```js
document.cookie='cookieName=cookieValue;expires=expireDate;path=/;domain=taobao.com'
```