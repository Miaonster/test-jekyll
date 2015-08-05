---
layout: post
title: "有哪些的隐藏内容的方法"
description: ""
category: 
tags: [Front-end-Developer-Interview-Questions, css]

---
{% include JB/setup %}

真的要隐藏的话，就是 `display: none;`，即便是读屏设备也会忽略掉这些内容。如果想要在读屏设备中让这些内容可见。解决方案的基本思路都是将这些内容放到屏幕、视线意外的地方，或者就是将大小设置成 0。比如 `text-indent: -9999em;`、`overflow: hidden;`、`height: 0`。 

更详细的方法可以参考这篇文章《[HIDING CONTENT FOR ACCESSIBILITY](http://snook.ca/archives/html_and_css/hiding-content-for-accessibility)》。

不过既然这是了读屏而优化的，那么可以用 media query 来完成，media speech 用于语音输出的读屏设备。

    @media speech {
        /* media-specify rules */
    }
