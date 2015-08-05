---
layout: post
title: "Thoughts about front-end framework: first"
description: ""
category: 
tags: [CSS]
---
{% include JB/setup %}

最近在做关于模块化的工作，简单记录一些不成文的想法。

## 一些想法

##### 前端框架其实包含了三个部分

* Javascript 框架
* CSS 样式框架
* html 模块化

##### CSS 样式框架，所涉及的范围

* CSS Reset
* 功能类
* iconFont
* Grid
* CSS3 Animate
* Common Style Library: box, button, form...
* Modules 
* ...

Javascript 框架

* 异步加载
* 依赖分析
* 模块化
* Widget
* MVC
* ...

##### HTML 模块

让 html 模块化，要在 server 分块输出 html，一个模版引擎是一个不错的选择。


## 如何写一个模块

一个 html 模块最终呈现需要三部分来完成，一是 html 本身，它由 MVC 以及模板引擎渲染后交由 WebServer 输出到用户的浏览器端；二是 html 本身会有 `data-widget＝wdigetName` 这样的属性用来表示由哪个 widget 来操作这一段 html；三是 html 对应的 CSS 样式，通过使用 class 来表示使用哪个模块的样式。

### 如何写一个 CSS 模块

我们的页面中总是存在的各式各样的模块化的 DOM 结构， 都可以依据其复用度来把其中一部分代码提炼成样式模块。模块包含了样式上所有可以通用的（或者是选择性通用的部分），更多的个性化的部分需要写在单独的 CSS 代码中。

形成的 CSS 模块应该会被放置在 src/widget 下面，使用的时候，有两种方式

* 直接引用

    ```
    <link rel="stylesheet" href="amd/widget/box.css">
    ```

* 在 less 文件中引用 box.less，可以使用下面这两种方式中的一种

    ```
    @import "src/widget/box.less";
    ```
    
    ```
    @import "src/widget/box.css";
    ```
    
### 如何写一个 Javascript 通用模块

由于使用了 g.js，我们可以很方便地通过使用 widget.js 来构造一个 Widget，一般的写法是在通过 `data-widget` 来指定需要加载的 widget 放在了何处，期望的用法是 

    <div data-widget="widget/box/box.js"></div>

### 如何写一个 HTML 模块

在 server 上输出 html 代码是很简单的事情，而将页面中的某一块模块化和前端模块化是两个问题，前端的 html 模块能做的是输出一段 html 代码，指定样式和动作（css & js）。而页面某一个部分的模块化，则是另一个话题了。

## 如何做到页面功能模块化

输出一个特定的模块到 webserver，而这个模块可以放在一个特定的位置，供任何页面来调用。期望的做法是放在这样的目录中：

    application/controllers/common/
    application/views/common/