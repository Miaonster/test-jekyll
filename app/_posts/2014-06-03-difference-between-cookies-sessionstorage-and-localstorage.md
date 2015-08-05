---
layout: post
title: "Cookies，SessionStorage 和 LocalStorage 的区别"
description: ""
category: 
tags: [Front-end-Developer-Interview-Questions, html]

---
{% include JB/setup %}

#### Cookie 

* 每个域名存储量比较小（各浏览器不同，大致4K）
* 所有域名的存储量有限制（各浏览器不同，大致4K）
* 有个数限制（各浏览器不同）
* 会随请求发送到服务器

#### LocalStorage
* 永久存储
* 单个域名存储量比较大（推荐5MB，各浏览器不同）
* 总体数量无限制

#### SessionStorage
* 只在 Session 内有效
* 存储量更大（推荐没有限制，但是实际上各浏览器也不同）