---
layout: post
title: "IE6 bug, float in cascade relative and absolute elements"
description: ""
category: 
tags: []
---
{% include JB/setup %}

今天在 IE6 下遇到了一个 bug。

需求是要做一个 tips，由一个图标的 hover 事件触发，显示出来这个tips，离开之后就消失。我希望用 `position: absolute;` 的方式来实现，而且弹出的层是在图标的基础上居中的，重点就在基于图标居中，于是我使用了这样的方式：

html:

    <div id="container">
		<div class="icon"></div>
		<div class="wrap">
			<div class="view">
		        this is the tip.
			</div>
		</div>
	</div>
	
css:

    .wrap {
    	position: absolute;
    	left: 50%;
    }
    
    .view {
    	left: -50%;
    	position: relative;
    	border: solid 1px #cccccc;
    }
    
预览：

<div id="container">
    <style type="text/css">
    #container {
        height: 120px;
    }
    .wrap {
    	position: absolute;
    	left: 50%;
    }
    
    .view {
    	left: -50%;
    	position: relative;
    	border: solid 1px #cccccc;
    	padding: 5px 10px;
    }
    </style>
	<div class="icon"></div>
	<div class="wrap">
		<div class="view">
	        this is the tip.
		</div>
	</div>
</div>

之后又在 tips 中加入了两个 `float:left` 的 div，见下面

html: 

    <div id="container">
		<div class="icon"></div>
		<div class="wrap">
			<div class="view">
				<div class="left">1</div>
				<div class="left">2</div
				<div style="clear: both;"></div>
			</div>
		</div>
	</div>
	
css:

    .wrap {
    	position: absolute;
    	left: 50%;
    }
    
    .view {
    	left: -50%;
    	position: relative;
    	border: solid 1px #cccccc;
    }
    
    .left {
    	float: left;
    	height: 100px;
    	width: 100px;
    	border: solid 1px #cccc00;
    	text-align: center;
    }
    
预览：

<div id="container">
    <style type="text/css">
    .left {
    	float: left;
    	height: 100px;
    	width: 100px;
    	border: solid 1px #cccc00;
    	text-align: center;
    }
    </style>
	<div class="icon"></div>
	<div class="wrap">
		<div class="view">
			<div class="left">1</div>
			<div class="left">2</div>
			<div style="clear: both;"></div>
		</div>
	</div>
</div>

但是呢，在 IE6 下，就会有个 bug，1 和 2 这两个 div 是从 外层 wrap 开始算的其实位置，于是就跑到了奇怪的位置去了，解决方式是，给 `float` 的 div 一个 `position: relative;`，感受一下：

html: 

    <div id="container">
		<div class="icon"></div>
		<div class="wrap">
			<div class="view">
				<div class="left">1</div>
				<div class="left">2</div
				<div style="clear: both;"></div>
			</div>
		</div>
	</div>
	
css:

    .wrap {
    	position: absolute;
    	left: 50%;
    }
    
    .view {
    	left: -50%;
    	position: relative;
    	border: solid 1px #cccccc;
    }
    
    .left {
    	float: left;
    	height: 100px;
    	width: 100px;
    	border: solid 1px #cccc00;
    	text-align: center;
        position: relative; /* <== here */
    }
    
预览：

<div id="container">
    <style type="text/css">
    .left {
    	float: left;
    	height: 100px;
    	width: 100px;
    	border: solid 1px #cccc00;
    	text-align: center;
    }
    </style>
	<div class="icon"></div>
	<div class="wrap">
		<div class="view">
			<div class="left">1</div>
			<div class="left">2</div>
			<div style="clear: both;"></div>
		</div>
	</div>
</div>