---
title: Chrome浏览器最小字体12px限制问题
date: 2016-10-28 17:24:27
tags: Skill
---
### 中文版Chrome浏览器不支持12px以下字体的解决方案
懒人直接拉到最下边看最终写法
Chrome 27之前的中文版桌面浏览器会默认设定页面的最小字号是12px，英文版则没有限制，主要是因为chrome认为汉字小于12px就会增加识别难度，尤其是中文常用的宋体和微软雅黑。而我们在实际项目中，对于数字/英文内容，其他字体的文本可能会有特殊的需求要求它们以更小的字号来显示，这个时候就需要取消浏览器的自动调整功能了。


一般解决方案是禁止webkit浏览器配置调整网页的字体大小。如下CSS定义方式：
```css
.classstyle{ -webkit-text-size-adjust:none; font-size:9px; } 
```

再讲一下text-size-adjust属性，该属性用来设定文字大小是否根据设备(浏览器)来自动调整显示大小，safari 3.0+，chrome 1.0+可以支持。属性值，可以为三种：
* percentage：字体显示的大小；
* auto：默认，字体大小会根据设备/浏览器来自动调整；
* none:字体大小不会自动调整


据说该属性最初专门是为iPhone版safari设计的，用来自动调整普通网页在iPhone手机端字体的展现问题，不过，既然是webkit的私有属性，现在也经常用在webkit内核的桌面浏览器限制页面展示。实际上，这是webkit的一个bug。在最新版的Chrome已经修复。

需要注意的是，不合理的使用-webkit-text-size-adjust:none会造成许多不好的影响。比如将其定义为全局属性的话，在Chrome中当用户放大缩小页面（PC上按住Ctrl滚动鼠标滚轮可缩放网页）的时候，文字却维持定义的大小而不放缩，给用户带来的不太友好的体验。所以我们在使用时，最好定义为局部有效，而不要图省事一句html{-webkit-text-size-adjust:none;}了事。


由于没有 -o-text-size-adjust，这样的CSS 属性，在 Opera，我们就只能通过自己手动设置了：工具->首选项->高级->字体->最小字体大小（像素）。

PC桌面版Chrome 27正式取消了-webkit-text-size-adjust属性的支持，实际上是修正了原有的bug。如果定义该属性在Chrome调试窗口会显示Unknown property name，所有字号最小为12px。那么现在该如何实现原来的需求呢？

*我们可以尝试通过对文字区域局部应用以下样式解决：*
```css
.chrome_adjust { 
font-size: 9px; 
-webkit-transform: scale(0.75); 
} 
```

12×0.75=9，对于其它浏览器来说，12px以下的字号都是可以识别的，这里仅需要对于Chrome浏览器进行缩放。可是如何分辨是Safari还是Chrome，目前尚没有有效的CSS hack。可以通过javascript来判断是否为Chrome。判断方法：var isChrome = !!window.chrome;当检测为Chrome的时候，将.chrome_adjust这个类添加到对应的标签。

但是，问题还没有解决。看到网页在三种浏览器的不同表现：
1. Chrome下由于启用了缩放，所以字符间距出现问题，影响了美观，这时候如果追求完美，可能你还会想到js判断为Chrome后再用CSS属性letter-spacing去修复；
2. Firefox不认识-webkit，所以表现正常，9px；
3. Opera 12.5+能够识别-webkit-前缀（Opera 12.15版本，内核暂未更换为webkit，但是已能够识别-webkit-前缀了，而且在检查元素时还抹掉了前缀），但又能够显示12px以下的字号，结果变成了9×0.75，影响了肉眼的识别，这时候，又得给opera添加-o-transform: scale(1);这个属性。

```css
.chrome_adjust { 
font-size: 9px; 
-webkit-transform: scale(0.75); 
-o-transform: scale(1); //针对能识别-webkit的opera browser设置 
} 
```

其实，源自挪威的Opera一贯使用自己的引擎(Presto)，但经常造成一些所谓的“兼容性问题”。在越来越多网站针对WebKit优化的情况下，Opera的对策就是，引入一些WebKit引擎的前缀属性，以避免开发者因为网页属性选择而影响Opera功能的发挥。Opera 12.50将是其第一个支持Webkit属性的桌面浏览器，包括-webkit-linear-gradient、-o-linear-gradient两种不同类型。移动版本也会使用同样的核心。相关测试的开发人员可以下载模拟器Opera Mobile Emulator——Windows、Linux、Mac版本。