---
layout: post
title: "Address Error when EnRoll Apple Developer"
description: ""
category: 
tags: [apple developer]

---
{% include JB/setup %}

今天注册 Apple 开发者的时候，遇到一个问题。第一步要填写账单地址，地址需要使用本地语言书写，比如：

    北京海淀区西三环北路某某大厦 B 座某层某某公司
    
这样的地址应该说是很常见的，但是点击提交的时候却报错说

    请用本地语言（中文）输入您的地址
    
我跟到页面的 javascript 代码里发现判断的条件是「如果有 a-zA-Z」就判断是非法地址。之后我就把「B 座」改成「2 号楼」就通过了。当然信用卡的账单地址也得跟着改，否则可能会被 reject。