---
title: css3动画简介以及动画库animate.css的使用
date: 2016-10-22 09:42:43
tags: Skill
---

### 过渡动画
第一种叫过渡（transition）动画，就是从初始状态过渡到结束状态这个过程中所产生的动画。所谓的状态就是指大小、位置、颜色、变形（transform）等等这些属性。css过渡只能定义首和尾两个状态，所以是最简单的一种动画。

要想使一个元素产生过渡动画，首先要在这个元素上用transition属性定义动画的各种参数。可定义的参数有:
1. transition-property：规定对哪个属性进行过渡
2. transition-duration：定义过渡的时间，默认是0
3. transition-timing-function：定义过渡动画的缓动效果，如淡入、淡出等，默认是 ease
4. transition-delay：规定过渡效果的延迟时间，即在过了这个时间后才开始动画，默认是0
```css
div{
    transition-property：width;       /*要参与过度的属性*/
    transition-duration：3s;          /*定义过渡的时间，默认是0*/
    transition-timing-function: ease; /*过度函数*/
    transition-delay: 3s;             /*动画在3s后开始*/
}
```
为了书写方便，也可以把这四个属性按照以上顺序简写在一个 transition 属性上：
```css
div{transition: width 3s ease 3s;}
```
如果是使属性的默认值，则可以省略
`div{transition: width 3s}`相当于`div{transitioin: width 3s ease 0;}`

如果要同时过度多个属性，可以用逗号隔开，或者直接写个all
```css
div{transition: width 3s,height 3s,background-color 3s;}

使用transtion属性只是规定了要如何去过渡，要想让动画发生，还得要有元素状态的改变。如何改变元素状态呢，当然就是在css中给这个元素定义一个类（:hover等伪类也可以），这个类描述的是过渡动画结束时元素的状态。
```css
div{
    width:100px;height:100px;background-color:#000;
    transition:all 3s ease;
}

div:hover{
    width:200px;height:200px;background-color:#fff;
}
```
这样，当我们把鼠标移动到div上的时候，div的状态发生了变化，就能看到宽度从100到400，高度从100到400，背景颜色从黑到红的，过渡时间为3秒的过渡效果了。
或者大多数时候我们使用js通过添加类来操作以实现动画效果：
```html
<style>
    div{
        width:100px; height:100px; background-color: #000;
        transition: all 3s ease 0;
    }
    div.on{width:300px;height:300px;background-color:#fff; }
</style>
<body>
    <div></div>
</body>
<script>
//直接用jQuery语法写了
    $('div').hover(function(){
        $('div').addClass('on')
    },function(){
        $('div').removeClass('on')
    })
</script>
```

### 关键帧动画
第二种叫做关键帧（keyframes）动画。不同于第一种的过渡动画只能定义首尾两个状态，关键帧动画可以定义多个状态，或者用关键帧来说的话，过渡动画只能定义第一帧和最后一帧这两个关键帧，而关键帧动画则可以定义任意多的关键帧，因而能实现更复杂的动画效果。

直接上实例写法：
```css
@keyframes demo{
    0%{background:red;width:100px}
    10%{background:green;width:200px}
    30%{background:yellow;width:300px}
    60%{background:pink;width:400px}
    80%{background:blue;width:500px}
    100%{background:orange;width:600px}
}
```
 这段代码定义了一个名为demo,且有5个关键帧的动画。0% ，10% 等这些表示的是时间点，是相对于整个动画的持续时间来说的，时间点之后的花括号里则是元素的状态属性集合，描述了这个元素在这个时间点的状态，动画发生时，就是从第一个状态到第二个状态进行过渡，然后从第二个状态到第三个状态进行过渡，直到最后一个状态。一般来说，0%和100%这两个关键帧是必须要定义的。

 关键帧的书写方式很灵活，一行可以写多个关键帧。甚至它们之间的空格也是可以不要的。


 现在我们知道了怎么去定义一个关键帧动画了，那怎么去实现这个动画呢？其实很简单，只要把这个动画绑定到某个要进行动画的元素上就行了。
 把动画绑定到元素上，我们可以使用animation属性。animation属性有以下这些：

| 属性          |解释     |
| ------------- |-------------|
| animation-name        | 规定keyframes动画名称    |
| animation-duration   | 一个周期耗时          |
| animation-timing-function   | 动画函数          |
| animation-delay   | 动画什么时候开始        |
| animation-iteration-count   | 动画播放次数，默认为1  infinite无限循环        |
| animation-direction   | 动画是否在下一周期逆向        |
| animation-play-state   | 动画是否在暂停或运行        |
| animation-fill-mode   | 规定对象动画之外的时间的状态        |

像前面讲的transition属性一样，也可以把这些animation属性简写到一个animation中，使用默认值的也可以省略掉。但 animation-play-state 属性不能简写到animation中。
```css
div{animation: demo 5s;}
```
只要像这样把定义好的动画绑定到元素上，就能实现关键帧动画了，而不是像第一种过渡动画那样，需要一个状态的改变才能触发动画。(注意，为了达到最佳的浏览器兼容效果，在实际书写代码的时候，还必须加上各大浏览器的私有前缀)
```css
<style>
@keyframes demo{
    0% {}
    20% {}
    40% {}
    60% {}
    100% {}
}
@-webkit-@keyframes demo{
    0% {}
    20% {}
    40% {}
    60% {}
    100% {}
}
@-moz-@keyframes demo{
    0% {}
    20% {}
    40% {}
    60% {}
    100% {}
}
@-o-@keyframes demo{
    0% {}
    20% {}
    40% {}
    60% {}
    100% {}
}
div{
    animation:demo 5s;
    -webkit-animation:demo 5s;
    -moz-animation:demo 5s;
    -o-animation:demo 5s;
}
</style>
```

### animate.css使用
animate.css是一个css3动画库，可以到github上去下载，里面预设了很多种常用的动画，可以先在本页看下演示效果，使用也很简单，因为它是把不同的动画绑定到了不同的类里，所以我们想要使用哪种动画的时候，只需要简单的把那个相应的类添加到元素上就行了：
首先在head中引入css文件
```html
<link rel="stylesheet" href="https://cdn.bootcss.com/animate.css/3.5.2/animate.min.css">
```
然后你想要哪个元素进行动画，就给那个元素添加上animated类 以及特定的动画类名，animated是每个要进行动画的元素都必须要添加的类。
```html
<h3 class="animated shake">animate.css</h3>
```

假设使用jquery，要给一个id为demo的元素添加一个摇动的动画,因为摇动的动画类名为shake，所以代码是这样的：
```js
$('#text').addClass('animated shake');
```
这样载入页面元素，元素就动了。也可以在动画完成后将动画关掉，以便可以再次进行同一个动画
```js
$('#text').addClass('animated shake');
setTimeout(function(){
    $('#text').removeClass('animated shake');
},3000) //假设这个动画持续3s
```
至于动画的配置参数，比如动画持续时间，动画的执行次数等等，可以在元素上自行定义，覆盖掉animate.css里面所定义的就行了(当然也可以直接去改源码)
```css
#text{
    animation-duration: 3s;
    animation-delay: 2s;
    animation-iteration: 2;
}
```
注意这些属性还要记得加上各浏览器的前缀。