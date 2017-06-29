---
title: flex简介
date: 2016-11-07 15:26:43
tags: Skill
---
Flex
flex是一种新的布局方案，可以简便、完整、响应式地实现各种页面布局。目前，它已经得到了所有浏览器的支持，这意味着，现在就能很安全地使用这项功能。
![浏览器支持](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071003.jpg)
对于移动端布局，flex非常方便。
webkit内核的浏览器在使用flex时必须加上前缀`-webkit-`
```css
.box{
    display:-webkit-flex;
    display:flex;
}
```
**注意：设为flex布局后，子元素的float，clear和vertical-align属性将失效**
### 一、基本概念
采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)
容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。
项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

### 二、容器(.box)的属性
1.确定盒子使用flex布局
```css
display : flex;
display : inline-flex;
```

2.flex-direction决定属性在主轴上的方向（即项目排列方向）
```css
flex-direction : column | column-reverse | row | row-reverse 
```
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071005.png)

3.flex-wrap定义如何换行
```css
flex-wrap : nowrap | wrap | wrap-reverse
```
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071009.jpg)

4.flex-flow是flex-direction属性和flex-wrap属性的简写，默认值row nowrap

5.justify-content项目在主轴上的对齐方式
```css
justify-content : flex-start | flex-end | center | space-between | space-around;
```
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png)

6.align-items定义项目在交叉轴上如何对齐
```css
align-items ： flex-start | flex-end | center | baseline | stretch
```
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png)

7.align-content定义了多跟轴线对齐方式，如果项目只有一根轴线该属性失效
```css
align-content : flex-start | flex-end | center | space-between | space-around | stretch
```

### 三、项目(.item)的属性
1.order 定义项目排列顺序，数值越小越靠前，默认为0
```css
order : 1;
```

2.flex-grow 定义项目的放大比例 默认为0，即如果存在剩余空间也不放大
```css
flex-grow : 1;
```
如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

3.flex-shrink 定义项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
```css
flex-shrink : 1;
```
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071015.jpg)
如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。

4.flex-basis 定义了在分配多余空间之前，项目占据的主轴空间，默认值为auto，即项目的本来大小
```css
flex-basis: <length> | auto; /* default auto */
```

5.flex属性是flex-grow,flex-shrink,flex-basis的缩写，默认值为0 1 auto。后两个值可选

6.align-self允许单个项目与其他项目有不同的对其方式，可覆盖align-items属性，默认auto，表示继承
```css
align-self: auto | flex-start | flex-end | center | baseline | stretch;
```
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071016.png)

> 来自[阮一峰博客](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)