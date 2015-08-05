---
layout: post
title: "Image Replacement"
description: ""
category: 
tags: [Front-end-Developer-Interview-Questions, css]

---
{% include JB/setup %}

Image Replacement 是说用图片把文字替换掉的技术，常用在标题上。

    <h3 class="h5bp">
      CSS-Tricks
    </h3>
    
    h3.h5bp {
      border: 0;
      font: 0/0 a;
      text-shadow: none;
      color: transparent;
      background: url(test.png);
      width: 300px;
      height: 75px;
    }
    
比如这个 H5BP 的替换方式是这样的，更多的可以看 css-tricks 的这个页面：[http://css-tricks.com/examples/ImageReplacement/](http://css-tricks.com/examples/ImageReplacement/)。