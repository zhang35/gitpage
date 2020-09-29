---
title: Bootstrap排版学习小结
date: 2018-11-24 11:21:41
tags: 
- bootstrap
description: Bootstrap学习笔记
categories: 前端开发 
---

> Bootstrap 包含了一个响应式的、移动设备优先的、不固定的网格系统

## 基本套路
```
<!DOCTYPE html>
<html>
    <head>
        <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" type="text/css" />
    </head>

    <body>
        <div class="container">
            <div class="row">
                <div class="col-*-*"></div>
                <div class="col-*-*"></div>      
            </div>
            <div class="row">...</div>
        </div>
        <div class="container">....
    </body>
    <!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
    <script src="https://cdn.staticfile.org/jquery/2.1.1/jquery.min.js"></script>
    <!-- 最新的 Bootstrap 核心 JavaScript 文件 -->
    <script src="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>    
</html>

```

下面一步步拆解。

## 指定doctype
指定HTML 版本，`<!DOCTYPE html>` 代指HTML 5。

> 如果在 Bootstrap 创建的网页开头不使用 HTML5 的文档类型（Doctype），您可能会面临一些浏览器显示不一致的问题，以致于您的代码不能通过 W3C 标准的验证。

## 添加meta标签
为了照顾不同设备的效果，添加 viewport meta 标签：

`<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">`

这些都是可选项。

> `width=device-width` 可以确保它能正确呈现在不同设备上。
> `initial-scale=1.0` 确保网页加载时，以 1:1 的比例呈现，不会有任何的缩放。
> `maximum-scale=1.0` 与 `user-scalable=no` 一起使用。这样禁用缩放功能后，用户只能滚动屏幕，就能让您的网站看上去更像原生应用的感觉。

详细参考：https://www.cnblogs.com/2050/p/3877280.html

## 引入bootstrap.css
单纯引入bootstrap.css后，页面内容立即有了变化，左边缘的文字已显示不全。

![引入bootstrap.css前](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTY0MDRlZmFmMTExOWQzYmUucG5n?x-oss-process=image/format,png)

![引入bootstrap.css后](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWYyODFlZDU1ODI5YjdlMDAucG5n?x-oss-process=image/format,png)

查看样式，也没找到答案。求解。
![没找到](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTczOTJjMThlZjgyNTI0ZjIucG5n?x-oss-process=image/format,png)


## 添加Container

用来框定bootstrap的有效范围。

把内容放入`<div class="container">`后，上一步遇到的页面显示不全问题就消失了。

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTQxZDNiMzllMjY4ODQ2NGIucG5n?x-oss-process=image/format,png)

也可替换为`<div class="container-fluid">`，具体参考：https://blog.csdn.net/weixin_42097173/article/details/80381896

## 添加row、col
> - 行必须放置在 .container class 内，以便获得适当的对齐（alignment）和内边距（padding）。
> - 使用行来创建列的水平组。
> - 内容应该放置在列内，且唯有列可以是行的直接子元素。

每一行自动分为12列，通过划分12个单位来排版。列数应 <= 12。
![bootstrap栅格系统](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTUyYmU0OWYyY2RkYjcyYmUucG5n?x-oss-process=image/format,png)

通过以下方式将元素嵌入网格：
```
<div class="row">
    <div class="col-*-*"></div>
    <div class="col-*-*"></div>      
</div>
```

| 类名     | 效果                                                         |
| -------- | ------------------------------------------------------------ |
| .col-xs- | 无论屏幕宽度如何，单元格都在一行，宽度按照百分比设置；试用于手机； |
| .col-sm- | 屏幕大于768px时，单元格在一行显示；屏幕小于768px时，独占一行；试用于平板； |
| .col-md- | 屏幕大于992px时，单元格在一行显示；屏幕小于992px时，独占一行；试用于桌面显示器； |
| .col-lg- | 屏幕大于1200px时，单元格在一行显示；屏幕小于1200px时，独占一行；适用于大型桌面显示器； |

具体参考：https://www.cnblogs.com/JerryTao/p/5476027.html

## 引入js文件
bootstrap的弹出框等组件包含在js中，也应一并引入。

在`bootstrap.min.js `之前，一定要引入`jquery.min.js`。

## 居中

文本居中：`.text-center`
元素居中：`.center-block`
列偏移居中：`col-xx-offset-x`（如下例中，内容宽6列，从左偏移3列后，布局变为3-6-3）

例如：
```
<div class="container">
            <div class="background center-block">
                <img class="img-responsive center-block" src="居中图片.jpg">
                <p class="text-center">居中文本</p>
                <div class="row" >
                    <div class="col-md-6 col-md-offset-3">
                         <p>偏移居中列</p>
                    </div>
                </div>
            </div>
 </div>

```