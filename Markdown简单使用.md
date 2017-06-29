---
title: Markdown的一些基础用法
date: 2016-10-20 09:36:04
tags: article
---



## 标题
标题是每篇文章都需要也是最常用的格式，在Markdown中要把一段文字定义为标题只需要在这段文字前加#号即可
![标题写法](http://cdn.sspai.com/attachment/thumbnail/2014/04/15/620e64aa6522f5eaeb788a8b5f1faa5c10f74_mw_800_wm_1_wmp_3.jpg)

## 插入图片与链接
图片详细叙述：
* 一个惊叹号
* 接着一组方括号，括号内写替代图片的文字
* 接着一个小括号，放图片链接地址
插入图片的地址需要图床，可以使用CloudAPP生成URL地址
![图片替代文字](http://cdn.sspai.com/attachment/thumbnail/2014/04/15/f96c892fc63933ab186235f7c910753b10f77_mw_800_wm_1_wmp_3.jpg)

链接插入：
    链接支持两种插入方式
    中括号内为显示的文字，小括号内为文字的链接

    This is [an example](http://www.baidu.com/ "Title") inline link.
    [This link](http://example.net/) has no title attribute.

## 列表
1. 无序标题：只需要在文字前加*或者-就可以了
2. 有序标题：在文字前加1.2.3.

![列表使用方法](http://cdn.sspai.com/attachment/thumbnail/2014/04/15/a72338b96cf4bfc1dacd610756786ae310f75_mw_800_wm_1_wmp_3.jpg)

## 引用
如果你要引用一段别处的句子，那么就需要用到引用的格式
> 例如这样

只需要在文本前加>就可以了
![引用使用方法](http://cdn.sspai.com/attachment/thumbnail/2014/04/15/07bd8bf6fd38ea7d3bffdc3cae04f6f210f76_mw_800_wm_1_wmp_3.jpg)

## 粗体与斜体
用两个*包含的文本是粗体，用一个*包含的文本是粗体
**粗体** *斜体*

## 代码框
如果你学要在文章里优雅的引用代码框，只需要用两个`，把中间的代码包起来就可以了。

`<h2>我是代码框,可以在这里写代码</h2>`

`<h3></h3>`

## 插入整段代码
    只需要将要插入的块整体缩进一下就可以了
    <html>
        <head></head>
        <body>
            <pre>
                <code>
                    写在pre  code两个标签内部会转为一个区块,
                    排版会按照我们写的文档的样式来排版而不是去自动排版。
                </code>
            </pre>
        </body>
    </html>

## 代码区块
    <pre>
        <code>
            写在pre  code两个标签内部会转为一个区块,
            排版会按照我们写的文档的样式来排版而不是去自动排版。
        </code>
    </pre>

##分割线
分割线只需要另起一行输入三个*就可以了
***

以上只是部分常用的Markdown语法，而这些也基本足够我们平时工作生活中使用了。如果想更详细的了解Markdown更多内容请查看官方文档[Markdown官方文档](http://www.appinn.com/markdown/)

## 这篇文章来自[这里](http://sspai.com/25137)的总结