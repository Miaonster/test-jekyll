---
layout: post
title: "Prevent right menu on Adobe Air HTML/Ajax"
description: ""
category: air
tags: []

---
{% include JB/setup %}

Air 的 HTML 开发模式中会有默认的右键菜单，在通常的浏览器中，我会选择 prevent right click，不过在 Air 上这个并不管用，右键之后还是会出现默认的菜单，过了一年我才想起来 contextmenu 事件。

屏蔽 contextmenu 就可以有效去处右键菜单了，下面是在 stackoverflow 上的[答案](http://stackoverflow.com/questions/13228628/adobe-air-htmljs-prevent-rightclick-event-of-image-input-and-textarea/16912255#16912255)，我在 jsfiddle 中也有一个 [demo](http://jsfiddle.net/Witcher42/VQWF7/)。

### Example:

Here is an <img> now.

    <img src="https://www.google.com/images/srpr/logo4w.png">
    
Add a contextmenu event listener for it, and prevent its default bebaviour.

    <img id="a" src="https://www.google.com/images/srpr/logo4w.png" >
    
    <script>
      $('#a').on('contextmenu', function(e) {
        e.preventDefault();
        e.stopPropagation();
      });
    </script>
    
Then the default menu disappears.