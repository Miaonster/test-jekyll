---
layout: post
title: "the difference between GET and POST"
description: ""
category: 
tags: [Front-end-Developer-Interview-Questions, html]

---
{% include JB/setup %}

#### 区别

* GET 将参数放在 URL 的 Query String 里，有长度限制，只支持 ASCII 数据
* POST 将参数放在 HTTP body 中

#### 引申

由此也可以引申出来一些问题：

* GET 会被浏览器缓存
* POST 会比 GET 安全一点点，当然最安全还是 HTTPS

#### 题外话

另外，HTTP 除了这两种请求方式，还有 HEAD、PUT、DELETE、OPTIONS、CONNECT、PATCH。

相关的话题就是 RESTful 接口，可以看看阮一峰的文章：《[理解RESTful架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)》和《[RESTful API 设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)》。