---
layout: post
title: "Prevent auto scroll when add new element"
description: ""
category: 
tags: [js]

---
{% include JB/setup %}

#Description

今天遇到一个问题，当将一个元素加入到一个很长的列表的时候，列表默认的动作是这样的：插入位置上方的元素不懂，下方的元素向下滚动。而有时候的需求却是反过来：下边的元素不动，上边的元素向上滚。


#Solution

在插入元素后滚动到正确位置，~~这个动作不会闪烁，因为 js 和渲染是“串行”的~~。计算 window 现在的 scrollTop 和插入元素的 outerHeight，二者的和就是需要重新滚动到的位置。可以看这个 jsfiddle 的 [Demo](http://jsfiddle.net/Witcher42/nB74K/2/)。

代码类似于这样

<pre>
<code class="javascript">
var scrollTop = $(window).scrollTop(),
    html = '&lt;div class="content"&gt; Content &lt;/div&gt;',
    $element = $(html).prependTo('#parent');

if (scrollTop){
    $(window).scrollTop(scrollTop + $element.outerHeight());
}

</code>
</pre>