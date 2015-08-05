---
layout: post
title: "浏览器标准模式和怪异模式之间的区别是什么"
description: ""
category: 
tags: [Front-end-Developer-Interview-Questions, html]

---
{% include JB/setup %}

如[前文](http://witcher42.github.io/2014/05/28/doctype)所说，浏览器渲染模式有三种：标准模式，准标准模式和混杂模式（怪异模式）。

* 标准模式（Standard Mode）是完全符合标准的模式。

* 准标准模式（Almost Standard Mode）是几乎要符合标准的模式。

* 混杂模式（Quirks Mode）是不符合 WEB 规范的模式。

在很久以前，网页会被写成两个版本：一个为 Netscape Navigator 而写，一个为 Microsoft Internet Explorer 而写。当 WEB 标准由 W3C 制定之后，浏览器却不能立刻使用这些标准，因为这么做会破坏绝大多数WEB 上已经存在的网页的呈现。于是浏览器使用了两种模式来处理符合新标准的页面和遗留的页面不兼容的问题。

现在的 WEB 浏览器的布局引擎使用三种模式：标准模式，准标准模式和混杂模式（怪异模式）。在混杂模式里，布局模拟 Navigator 4 和 Internet Explorer 5 for Windows 中非标准的行为，以免破坏已经在这些浏览器中的存在的内容。在标准模式中，布局的行为（但愿）和规范中描述的一致。在准标准模式中，只有非常少的怪异行为。

在 Quirks Mode 中具体的不同可以看这个页面：[https://www.cs.tut.fi/~jkorpela/quirks-mode.html](https://www.cs.tut.fi/~jkorpela/quirks-mode.html)。

这里简单挑两个来看一看：

* 盒子模型是错的（与 CSS 规范不同，虽然直观上讲或许会更自然一些），意思是说，width 和 height 属性指定的整个元素盒子的尺寸，包含了 padding 和 border，也就是说不只是元素的 content 而已（作者按：就像是 `box-sizing: border-box;`）。

    > The **box model** is incorrect (different from CSS spec­i­fi­ca­tions, though perhaps intuitively more natural). This means that the width and height properties specify the dimen­sions of the entire element box, including padding and border, and not just the element’s content. (There is a demo later in this document.)
   
* non-replaced inline 元素（比如默认是 span）的尺寸，会受到 width 和 height 的作用（虽然 CSS 规范中这两个属性应当被忽略）

    > **Dimensions** of non-replaced inline elements (e.g., span elements by default) are affected by width and height properties (while by CSS specifications, they shall be ignored).