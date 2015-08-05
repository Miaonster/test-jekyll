---
layout: post
title: "simulate autocompletion of QQ login box"
description: ""
category: 
tags: [js, jquery]
---
{% include JB/setup %}

现在有需求是用 js 来做 QQ 登陆框中的用户名自动补全的功能，我开始做这个功能的时候有这么几个思路。

### Selection & Range

js 中有 Selection 和 Range 结构，这两个结构搭配起来可以设定鼠标在文本中的位置、或是选中文本，最初的想法是在 input 或者是 keydown 的时候来将 input 的 value 补全，再选取剩余的文本，但是实际发现用户的操作速度远比 input 事件的处理速度要快，而且在输入的同时操作 value 总是会有奇怪的事情发生，比如光标忽然跑到最后去了、选中的文本没有选中了。这种方式应该是不行了，于是我又想到了第二种方式。

### window.setTimeout

最先想到的是可不可以使用定时器来做，用户没有操作的一段时间之后再去改变 value，这个想法大多数时间下都是好的，但是在不算少的情况下也会出现上一条说的问题，这也是同时操作 value 导致的。

### jquery.autocompletion.js

于是我写了一个 jquery 插件，专门来做这件事情，大体的思路是将 input 下面叠上一个 span，span 显示完整的文本，将样式设为选中的感觉，这样露出来的就是需要补全的部分。

而且这样设置之后就将输入部分和需要补全的部分分开，不会因为输入速度的问题影响对补全部分的处理。

看看 [demo](http://jsfiddle.net/Witcher42/dYCxh/)，感受一下。

Fork me on Github [https://github.com/Witcher42/jquery.autocompletion.js](https://github.com/Witcher42/jquery.autocompletion.js)。
