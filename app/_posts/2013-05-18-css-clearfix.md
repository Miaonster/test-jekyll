---
layout: post
title: "CSS clearfix"
description: ""
category: 狮子奋疾三昧
tags: [CSS, 每日一练, 狮子奋疾三昧]

---
{% include JB/setup %}

#Clearing floats

工作之后，自己半路出家学了js，没有系统的学习过 html/css/js 这一套体系，每每细想一个问题的深层原因的时候总觉得没有底气，于是打算从基础开始系统地梳理一遍相关知识。徐公子的《惊门》中最近正好讲到了“狮子奋疾三昧”的功法，说的是从破妄大成的境界重回色欲劫的开端，再重新印证至原来境界。重历来时道路应当别有所得，这也应当能印证到自己这一段时间的经历中来。

另外，我还打算开启“每日一练”的进程，希望日日能有所得。

在 [codescholl](http://codescholl.com) 的课程上看到了关于clearfix的问题。写了两年js，完全没有遇到过这种问题。这里讲的是一个普遍存在的需求：float 元素的 container 的 border 不会包括里面的 float 元素， 但是我们却想实现这个功能。于是引入了 `clear: left / right / both;` 这个属性，以及 `:after` 伪元素，让我们可以撑起 container 元素。

# 问题描述

先来看一下问题是如何发生的，css 列表如下：

    div.container {
    	border: 1px solid #000000;
    }
    
    div.left {
    	width: 45%;
    	float: left;
    }
    
    div.right {
    	width: 45%;
    	float: right;
    }

HTML 代码：

     <div class="container">
    	<div class="left">
    		<p>{ float: left; }</p>
    	</div>
    	<div class="right">
    		<p>{ float: right; }</p>
    	</div>
    </div>

如此，我们就可以看到，因为 div.container 并没有 height 属性，所以它的 border 是没有被撑起来的。


<div>
    <style type="text/css">
    .left p,.right p{
        margin-top: auto;
        margin-bottom: auto;
    }
    div.container {
    	border: 1px solid #000000;
    }
    
    div.left {
    	width: 45%;
    	float: left;
    	text-align: center;
    	background-color: #BADA55;
    	color: #333;
    }
    
    div.right {
    	width: 45%;
    	float: right;
    	text-align: center;
    	background-color: #BADA55;
    	color: #333;
    }
    </style>
    <div class="container">
    	<div class="left">
    		<p>{ float: left; }</p>
    	</div>
    	<div class="right">
    		<p>{ float: right; }</p>
    	</div>
    </div>
</div>

<div style="clear:both;"></div>

# 解决方案

我们的目标是让 border 将它里面的 float 元素包裹住，教程里给出了两种方案.

### clear: left / right / both; 

在 float 元素之后加上一个 div 后者 br 标签，并且为其增加 clear 样式，CSS 代码列表如下：

    div.container {
    	border: 1px solid #000000;
    }
    
    div.left {
    	width: 45%;
    	float: left;
    }
    
    div.right {
    	width: 45%;
    	float: right;
    }
    
    div.clear {
        clear: both;
    }

HTML 代码：
    
    <div class="container">
    	<div class="left">
    		<p>{ float: left; }</p>
    	</div>
    	<div class="right">
    		<p>{ float: right; }</p>
    	</div>
    	<div class="clear"></div>
    </div>

可以看到，此时的 .containter 的 border 已经环绕了内部的两个 div.left 和 div.right。

<div>
    <style type="text/css">
    div.container {
    	border: 1px solid #000000;
    }
    div.left {
    	width: 45%;
    	float: left;
    	text-align: center;
    	background-color: #BADA55;
    	color: #333;
    }
    div.right {
    	width: 45%;
    	float: right;
    	text-align: center;
    	background-color: #BADA55;
    	color: #333;
    }
    div.clear {
        clear: both;
    }
    </style>
<div class="container">
<div class="left">
<p>{ float: left; }</p>
</div>
<div class="right">
<p>{ float: right; }</p>
</div>
<div class="clear"></div>
</div>
</div>
<div style="clear:both;"></div>
<br>

### 伪元素 clearfix:after

在 [codescholl](http://cssxcountry.codeschool.com/levels/2#) 的课程中，给出了第二种方式，就是使用伪元素 :after 来实现，CSS代码如下：

    div.container {
    	border: 1px solid #000000;
    }
    
    div.left {
    	width: 45%;
    	float: left;
    }
    
    div.right {
    	width: 45%;
    	float: right;
    }
    
    .clearfix:before,
    .clearfix:after {
        content: "";
        display: table;
    }
    .clearfix:after {
        clear:both;
    }
    .clearfix {
        zoom: 1; /* IE6 & 7 */
    }

HTML 代码如下：   

    <div class="container clearfix">
    	<div class="left">
    		<p>{ float: left; }</p>
    	</div>
    	<div class="right">
    		<p>{ float: right; }</p>
    	</div>
    </div>

于是，效果如下：

<div>
    <style type="text/css">
    
    div.container {
    	border: 1px solid #000000;
    }
    
    div.left {
    	width: 45%;
    	float: left;
    	text-align: center;
    	background-color: #BADA55;
    	color: #333;
    }
    
    div.right {
    	width: 45%;
    	float: right;
    	text-align: center;
    	background-color: #BADA55;
    	color: #333;
    }
    
    .clearfix:before,
    .clearfix:after {
        content: "";
        display:table;
    }
    .clearfix:after {
        clear:both;
    }
    .clearfix {
        zoom: 1; /* IE6, IE7 */
    }    </style>
    <div class="container clearfix">
    	<div class="left">
    		<p>{ float: left; }</p>
    	</div>
    	<div class="right">
    		<p>{ float: right; }</p>
    	</div>
    </div>
</div>

# 余韵悠然

问题似乎已经被解决了，但是问题在于，为何这样会解决呢，原因在哪里？这里面似乎涉及了很多复杂的原理。

* { zoom: 1 } 是对 IE 中 hasLayout 的触发，那么什么是 hasLayout呢？
* { display: table; } 涉及到了叫做 BFC (Block Formatting Contexts) 或者是 Flow Root 的东西，BFC 是在 CSS 2.1 中出现的东西，而 Flow Root 是 CSS3 的规范，看上去还挺复杂的。

上面两点以及更加复杂的原因可以看[这篇博客](http://www.iyunlu.com/view/css-xhtml/55.html)，详细解读了“浮动闭合”的各种解决方案以及其中的深层原理，令人所获颇多。

至于其他方面的问题，我似乎还无从知晓，开始学习课程只到第二节，更多知识还要融会贯通之后再慢慢拆解了。