---
layout: post
title: "Native click won't be triggered by jQuery"
description: ""
category: 
tags: [js, jquery]
---
{% include JB/setup %}

一般来讲，直接 trigger 一个 `a` 标签的 `click` 事件是不被浏览器允许的，正常情况下都是由用户的 `click` 事件来触发一个「点击动作」，在这个「动作」中，我们可以调用触发其他的 `click` 事件，就好像用户点在了其他的标签上。按照长期使用 jQuery 的思路，一般就会直接使用下面的方式来触发：

    $('a').click();

或者是：    

    $('a').trigger('click');

很遗憾的是，这样是不行的，jQuery 并不会为 `a` 标签提供 *native click* 事件的触发，而我们想要做这件事情就需要用原生的 *DOM element* 来做：

    $('a').get(0).click();
    
至于为什么？我找到了一个说法，是 Learn jQuery 上的一篇文章：[Triggering Event Handlers](http://learn.jquery.com/events/triggering-event-handlers/)。它说到：

> The `.trigger()` function cannot be used to mimic native browser events, such as clicking on a file input box or an anchor tag.

> This is because, there is no event handler attached using jQuery's event system that corresponds to these events.

`.trigger()` 函数不能用来模拟 native browser events，比如 click 一个 **file input box** 或是 **a** 标签。

这是因为，jQuery 的 event system 中没有对应于这些事件的 event handler。

### Why

怎么理解呢？我在 *stackoverflow* 上问了同样的问题：[Why jQuery cannot trigger native click on an anchor tag](http://stackoverflow.com/questions/20782534/why-jquery-cannot-trigger-native-click-on-an-anchor-tag)。

得到了这样的答案：

> .trigger()
> 
> Execute all handlers and behaviors attached to the matched elements for the given event type.

简单说来，就是从 jQuery 的视角来看，`.trigger()` 只会去执行所有通过 jQuery 注册的函数。这看上去很有道理，答案还有附有一个 [jsFiddle](http://jsfiddle.net/f9vkd/14/)，可以看到，使用 native 方法绑定的事件也是不会被 jQuery 调用的（显然不会……）。

所以问题的答案就是这样，jQuery 不会去触发没有通过 jQuery 注册的函数，而点击链接所伴随的打开新窗口的事件是类似于一种 *behaviors* 的存在。