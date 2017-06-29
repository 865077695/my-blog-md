---
title: call-apply
date: 2016-11-14 18:00:01
tags: Skill
---

### 定义
call方法: 
语法：call([thisObj[,arg1[, arg2[,   [,.argN]]]]]) 
定义：调用一个对象的一个方法，以另一个对象替换当前对象。 
说明： 
call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。 
如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。 

apply方法： 
语法：apply([thisObj[,argArray]]) 
定义：应用某一对象的一个方法，用另一个对象替换当前对象。 
说明： 
如果 argArray 不是一个有效的数组或者不是 arguments 对象，那么将导致一个 TypeError。 
如果没有提供 argArray 和 thisObj 任何一个参数，那么 Global 对象将被用作 thisObj， 并且无法被传递任何参数。

### 通过例子看各自作用及不同之处
```js
// 看完了再回头看看这句话：apply方法能劫持一个对象的方法，继承另外一个对象的属性.
var func=new function(){
    this.a="func"
}
var myfunc=function(x,y){
    var a="myfunc";
    console.log(this);  
    console.log(this.a);
    console.log(x);
    console.log(y);
}
myfunc("var","zh");     //调用myfunc此时的this是window，因此this.a为undefined
myfunc.call(func,"var","zh");       //此时在调用myfunc方法时将该方法中的this从window替换成了func,而func.a明显是"func"
myfunc.apply(func,["var","zh"]);    //相比较call，两者只是参数的传入方式不一样，call是一个一个的字串，而apply只是将这些字串放在一个数组中传入。
//如：func.call(func1,var1,var2,var3)对应的apply写法为：func.apply(func1,[var1,var2,var3])
```

同时使用apply的好处是可以直接将当前函数的arguments对象作为apply的第二个参数传入
```js
function Person(name,age){
    this.name = name;
    this.age = age;
}
function student(name,age,grade){
    // Person.call(this,name,age)
    // Person.apply(this,[name,age,grade]);
    Person.apply(this,arguments)        //在这里这行代码与上面两行代码作用一样
    this.grade = grade;
}
var student = new student('zhq','24','一年级');
console.log(student.name+'-'+student.age+'-'+student.grade);            // zhq-24-一年级
```
分析: Person.apply(this,arguments);
this:在创建对象在这个时候代表的是student
arguments:是一个数组,也就是[zhq,”24”,”一年级”];
也就是通俗一点讲就是:用student去执行Person这个类里面的内容,在Person这个类里面存在this.name等之类的语句,**这样就将属性创建到了student对象里面**
再贴一遍这句话：apply方法能劫持一个对象的方法，继承另外一个对象的属性.
### apply的其他一些妙用
可以看到,在调用apply方法的时候,第一个参数是对象(this), 第二个参数是一个数组集合, 在调用Person的时候,他需要的不是一个数组,但是为什么给他一个数组他仍然可以将数组解析为一个一个的参数,这个就是apply的一个巧妙的用处,可以将一个数组默认的转换为一个参数列表([param1,param2,param3] 转换为 param1,param2,param3) 
因此当一个方法必须传入一个个字串才能执行，而我们手里的是一个数组，这时候如果让我们用程序来实现将数组的每一个项,来转换为参数的列表,可能都得费一会功夫,借助apply的这点特性,所以就有了以下高效率的方法:

* Math.max 可以实现得到数组中最大的一项
因为Math.max 参数里面不支持Math.max([param1,param2]) 也就是数组,但是它支持Math.max(param1,param2,param3…),所以可以根据刚才apply的那个特点来解决 
```js
var max=Math.max.apply(null,array)
```
这样轻易的可以得到一个数组中最大的一项(apply会将一个数组装换为一个参数接一个参数的传递给方法)
这块在调用的时候第一个参数给了一个null,这个是因为没有对象去调用这个方法,我只需要用这个方法帮我运算,得到返回的结果就行,.所以直接传递了一个null过去
* Array.prototype.push 可以实现两个数组合并
同样push方法没有提供push一个数组,但是它提供了push(param1,param,…paramN) 所以同样也可以通过apply来装换一下这个数组,即:
```js
Array.prototype.push.apply(arr1,arr2);  
```

可以这样帮助理解，一般我们都是要用一个对象去调用一个方法，而通过call和apply好像我们用一个方法在呼叫一个对象