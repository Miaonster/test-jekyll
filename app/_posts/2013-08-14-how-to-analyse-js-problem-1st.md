---
layout: post
title: "How to analyse Javascript problems"
description: ""
category: 
tags: [js]

---
{% include JB/setup %}

這不是教程，而是 JS 的分析記錄。

現在的背景是這樣的：不能很方便地審查元素所屬 style，以及該 style 是如何一層層地覆蓋來的。需要定位的問題是，為什麼滾動條會在底部顯示成多餘的滾動條箭頭。此時的元素卻沒有多餘的 padding 或是 maring，那麼是為什麼呢？

* 定位到該元素
* 寫一個簡單的類似的元素，看是否不同
* 發現簡單的是對的，複雜的卻是錯的
* 那麼差別在哪裡呢？是子元素的數量還是子元素的長度？
* 發現是子元素的長度，改為 `overflow:hidden;` 即可。

<small>用繁體是因為當前用的字體對簡體的解析不正確，待老夫有時間了修改一下<small>