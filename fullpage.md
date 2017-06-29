---
title: FullPage插件使用
date: 2016-10-21 17:25:12
tags: Skill
---

> 来自于窝窝看视频教程所做的笔记

一样直接进入主题。

fullpage是一个基于jQuery的插件，它能够很方便、很轻松的制作出全屏网站，主要功能有：
* 支持鼠标滚动
* 支持前进后退键盘控制
* 多个回调函数
* 支持手机平板触摸事件
* 支持css3动画
* 支持窗口缩放
* 窗口缩放时自动调整
* 可设置滚动宽度、背景颜色、滚动速度、循环选项、回调、文本对齐方式等等

## 使用方法
### 引入文件
这里直接粘贴出免费cdn公共库地址，也可以选择将文件放到自己的服务器

`https://cdnjs.cloudflare.com/ajax/libs/fullPage.js/2.7.9/jquery.fullPage.min.css` 样式
`https://cdnjs.cloudflare.com/ajax/libs/fullPage.js/2.7.9/jquery.fullPage.min.js` js文件
`https://cdnjs.cloudflare.com/ajax/libs/fullPage.js/2.7.9/vendors/jquery.easings.min.js` easings插件，提供一些动画效果
`https://cdnjs.cloudflare.com/ajax/libs/fullPage.js/2.7.9/vendors/jquery.slimscroll.min.js` slimscroll插件  实现页面中内容超长的一个滚动效果必须要引入的文件
### 页面基本结构
```html
        <div id="fullpage">
            <div class="section">一些内容</div>
            <div class="section">一些内容</div>
            <div class="section">一些内容</div>
            <div class="section">一些内容</div>
        </div>
```
### 给某个section增加slide幻灯片
```html
<div class="section">
    <div class="slide">slide 1</div>
    <div class="slide">slide 2</div>
    <div class="slide">slide 3</div>
    <div class="slide">slide 4</div>
</div>
```
### 激活fullpage效果
```js
$(document).ready(function(){
    $('#fullpage').fullpage();
})  
```
### 配置项
配置项作为fullpage()的参数对象的属性如下方式写入
```js
$(document).ready(function(){
    $('#fullpage').fullpage({
        sectionsColor:['green','orange','gray','pink'],
        controlArrows:false,
        afterLoad:function(link,index){
                console.log(link+'---'+index)              
            }
    });
}) 
```
1.sectionsColor:为每一个section设置background-color属性。sectionsColor:['green','orange','gray','pink']
2.controlArrows:定义是否通过箭头来控制slide幻灯片，默认为true。取消箭头用false。controlArrows:false
3.verticalCentered:每一页的内容是否垂直居中，默认为true。verticalCentered:false
4.resize:字体是否随着窗口缩放而缩放，默认为false。  resize:true,
5.scrollingSpeed:滚动速度，单位为毫秒，默认为700   scrollingSpeed:2000
6.anchors:定义锚链接，默认值为[]。用户可以快速打开定位到某一页面。注意定义锚链接的时候，值不要和页面中任意的ID或name相同，尤其在ie浏览器下。而且定义时不需要加#。
anchors:['page1','page2','page3','page4'],还有class:"active"将active设置在哪，浏览器就会打开第几屏
7.lockAnchors:是否锁定锚链接，默认为false。如果设置为true，那么定义的锚链接没有效果
8.easing:定义页面section滚动的动画方式，默认为easeInOutCubic
9.css3:是否使用css3 transforms来实现滚动效果，默认为true。这个配置项可以提高支持css3的浏览器或者是移动端的效果和速度，如果浏览器不支持css3，则会使用jquery来替代css3实现滚动效果（优雅降级）
10.loopTop:滚动到最顶部后是否连续滚动到底部，默认false   loopTop:true
11.loopBottom:滚动到最底部后是否连续滚动回顶部    loopBottom:true
12.loopHorizontal:横向slider幻灯片是否循环滚动，默认为true  loopHorizontal:true
13.autoScrolling:是否使用插件的滚动方式，默认为true，如果选择false，则会出现浏览器自带的滚动条，将不会按页滚动，而是按照滚动条的默认行为来滚动
14.scrollBar:是否包含滚动条，默认为false，如果设置为true，则浏览器自带的滚动条出现，页面滚动时还是按页滚动，但是滚动条的默认行为也是有效的(将滚动条拖动的时候)
15.paddingTop/paddingBottom:设置每一个section顶部和底部的padding，默认为0；paddingTop/paddingBottom:0,一般如果我们需要设置一个固定在顶部或者底部的菜单、导航、元素等，可以使用这两个配置项
16.fixedElements:固定元素，默认为null，需要配置一个jquery选择器，在页面滚动时，其设置的元素固定不动，也就是说他不会随页面的滚动而滚动。
fixedElements:'#header',     如果不设置，header会被覆盖掉，并且看不到
17.keyboardScrolling:是否使用键盘方向键导航，默认为true
18.touchSensitive:在移动设备中滑动页面的敏感性，默认为5，是按百分比来衡量。最高为100， 越大越难滑动
19.continuousVertical:是否循环滚动，默认为false，如果设置为true，则页面会循环滚动，而不像loopTop或loopBottom那样出现跳动，注意这个属性loopTop或loopBottom不兼容，不要同时设置
20.animateAnchor:锚链接是否可以控制滚动动画，默认为true。为false，则通过锚链接定位到某个页面显示不再有动画效果
21.recordHistory:是否记录历史，默认为true，可以记录页面滚动的历史，通过浏览器的前进后退来导航。如果设置了autoScrolling：false，那么这个配置也将关闭。recordHistory:false
22.menu:绑定菜单。设定的相关属性与anchors的值对应后，菜单可以控制滚动，默认为false,可以设置为菜单的jquery选择器
```html
<ul id="fullpageMenu">  <!-- 设置菜单第一步： 用ul标签写,并且设置id名为fullpageMenu-->
      <!--菜单第二步： <a href="#page1"></a>菜单的链接指向每一个锚链接，也就是说和每一个section
     【anchors:['page1','page2','page3','page4'],】里面的锚链接的设置是一一对应的 -->
     <!--菜单第三步： 还需设置data-menuanchor="page1"这样一个属性，他也和下面每一个section中设置的锚链接一一对应
    【anchors:['page1','page2','page3','page4'],】 -->
     <li data-menuanchor="page1" class="active"><a href="#page1">首页第一屏</a></li>
     <li data-menuanchor="page2"><a href="#page2">第二屏</a></li>
     <li data-menuanchor="page3"><a href="#page3">第三屏</a></li>
     <li data-menuanchor="page4"><a href="#page4">第四屏</a></li>
</ul>
// 定义锚链接，默认值为[]。用户可以快速打开定位到某一页面,当定义菜单的时候，要设置锚链接
anchors:['page1','page2','page3','page4'],
// 菜单第四步：
menu:'#fullpageMenu',
```
23.navigation:是否显示导航，默认为false，如果设置为true，会显示小圆点，作为导航。navigation:true
24.navigationPosition:导航小圆点的位置，可以设置为left或者right。  navigationPosition:right
25.navigationTooltips:导航小圆点的tooltips设置，默认为[]，注意按照顺序设置  navigationTooltips:['page1','page2','page3','page4']
26.showActiveTooltip:是否显示当前页面的导航的tooltip信息，默认为false。showActiveTooltip:true
27.slidesNavigation:是否显示横向幻灯片的导航，默认为false  slidesNavigation:true
28.slidesNavPosition:横向幻灯片导航的位置，默认为bottom,可以设置为top或bottom   slidesNavPosition:top
29.scrollOverflow:内容超过满屏后是否显示滚动条，默认为false。如果设置为true,则会显示滚动条，如果滚动查看内容，还需要jquery.slimscroll插件的配合，slimscroll插件主要用于模拟传统的浏览器滚动条   
`scrollOverflow:true`,  当设置的某一屏的内容超出之后，需要这样设置，才能看全某屏的所有内容
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jQuery-slimScroll/1.3.7/jquery.slimscroll.js"></script>
<div class="section">这是第三屏
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
    <p>内容超出，需设置scrollOverflow:true,还需要引入jQuery-slimScroll插件才可以一在当前屏滚动的方式查看超出部分的内容</p>
</div>
```
30.sectionSelector:section的选择器，默认为.section 
31.slideSelector:slide的选择器，默认为.slide
### 方法  $.fn.fullpage.XXX()
写法eg（轮播自动播放）：
```js
         setInterval(function(){
            $.fn.fullpage.moveSlideRight();
        }, 3000);
```
1. moveSectionUp:       向上滚动一页
2. moveSectionDown:   向下滚动一页    
3. moveTo(section,slide):   滚动到第几页，第几个幻灯片，注意，页面是从1开始，而幻灯片是从0开始 
$.fn.fullpage.moveTo(2,2)   滚动到第二页，第三个幻灯片
4. slideMoveTo(section,slide):  滚动到第几页，和moveTo一样，但是没有动画效果
5. moveSlideRight():       幻灯片向右滚动
6. moveSlideLeft():          幻灯片向左滚动
7. setAutoScrollling(boolean):    动态设置autoScrolling
8. setLockAnchors(boolean):    动态设置lockAnchors
9. setRecordHistory(boolean):    动态设置recordHistory
10. setScrolllingSpeed(millseconds):    动态设置scrolllingSpeed
11. setAllowScrolling(boolean,[directions]):  添加或者删除鼠标滚轮/滑动控制，第一个参数true为启用，false为禁用
后面的参数为方向，取值包含all,up,down,left,right,可以使用多个，逗号分隔
eg:`$.fn.fullpage.setAllowScrolling(true,'down')`
作用：比如我们在做一个有奖问答页面，提问的问题在前面的页面有答案，当滚动到最后一页时，不希望用户在滚动回之前的页面查看答案，就可以使用这样的方法
12. destroy(type):销毁fullpage特效，type可以不写，或者使用all，不写type，fullpage给页面添加的样式和html元素还在，如果使用all，则样式、html等全部销毁，页面恢复和不使用fullpage相同的效果
`$.fn.fullpage.destroy('all')`
13.reBuild():重新更新页面和尺寸，用于通过ajax请求后改变了页面结构之后，重建效果
### 回调函数
回调函数作为对象的方法传入
1. afterload(anchorLink,index)  滚动到某一section，且滚回结束后会触发一次此回调函数，函数接受
anchorLink和index两个参数，anchorLink是锚链接的名称，index是序号，从1开始计算
我们可以根据anchorLink和index参数值的判断，触发相应的事件
```js
afterload:function(anchorLink,index){
     console.log("afterload: anchorLink="+anchorLink+";index="+index);
}
```
2. onLeave(index,nextIndex,direction)
在我们离开一个section时，会触发一次此回调函数，接收index、nextIndex和direction 3个参数
index是离开的“页面”的序号，从1开始计算；
nextIndex是滚动到的目标“页面的序号，从1开始计算”；
direction判断往上滚动还是往下滚动，值为up或down
通过return false;可以取消滚动
```js
onLeave:function(index,nextIndex,direction){
     console.log("onLeave: index="+index+";
     nextIndex="+nextIndex+";
     direction="+direction);
}
```
3. afterRender():页面结构生成后的回调函数，或者说页面初始化完成后的回掉函数
4. afterResize() 浏览器窗口尺寸改变后的回调函数
5. afterSlideLoad(anchorLink,index,slideAnchor,slideIndex)滚动到某一幻灯片后的回调函数，与afterLoad类似，接收anchorLink,index,slideIndex,direction4个参数
6. onSlideLeave(anchorLink,index,slideIndex,direction,nextSlideIndex)
在我们离开一个slide时，会触发一次此回调函数，与onLeave类似,接收anchorLink,index,slideIndex,direction4个参数