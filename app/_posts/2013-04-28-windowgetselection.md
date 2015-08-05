---
layout: post
title: "window.getSelection"
description: ""
category: javascript
tags: [javascript]
---
{% include JB/setup %}

Summary
---
返回一个selection对象，该对象表示由用户选取的文本范围。

Syntax
---
    selection = window.getSelection() ;

* `selection` 是一个 [Selection](https://developer.mozilla.org/en-US/docs/DOM/Selection) 对象.

Example
---
    function foo() {
      var selObj = window.getSelection(); 
      alert(selObj);
      var selRange = selObj.getRangeAt(0);
      // do stuff with the range
    }
    
Notes
---
在Javascript中，当一个 Object 被传入一个需要 String 参数的函数中（像 [window.alert](https://developer.mozilla.org/en-US/docs/DOM/window.alert) 或者 [document.write](https://developer.mozilla.org/en-US/docs/DOM/document.write) ），默认会调用该 Object 的 toString() 方法，并且把返回值传入该函数中。