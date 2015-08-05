---
layout: post
title: "data- 属性的作用是什么"
description: ""
category: 
tags: [Front-end-Developer-Interview-Questions, html]

---
{% include JB/setup %}

当没有合适的属性和元素时，自定义的 data 属性是能够存储页面或 App 的私有的自定义数据。

> Custom data attributes are intended to store custom data private to the page or application, for which there are no more appropriate attributes or elements.

可以通过 `ele.dataset.xxx` 来访问 `data-xxx=""`。