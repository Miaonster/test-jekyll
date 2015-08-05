---
layout: post
title: "CSS sprites"
description: ""
category: 
tags: [Front-end-Developer-Interview-Questions, css]

---
{% include JB/setup %}

CSS Sprites 是把图片都合到一张大图上，这么做的目的是为了减少图片的 HTTP 请求数。

具体实施的时候，让设计师去拼总是很有问题，还是要用工具来构建项目，比如「[css-spirte](https://github.com/aslansky/css-sprite)」，「[node-sprite](https://github.com/naltatis/node-sprite)」，还有「 [Compass 的 sprite](http://compass-style.org/help/tutorials/spriting/)」。合并图片的工具的思路就是，用合并后的图片的位置和 CSS 中使用的位置，来算出新的位置，生成新的图片和 CSS 文件。