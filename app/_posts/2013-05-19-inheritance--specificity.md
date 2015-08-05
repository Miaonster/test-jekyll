---
layout: post
title: "CSS Inheritance & Specificity"
description: ""
category: 狮子奋疾三昧
tags: [CSS, 每日一练, 狮子奋疾三昧]

---
{% include JB/setup %}

今天的 [codeschool 课程](http://cssxcountry.codeschool.com/levels/2#) 讲的是 CSS 的优先级，课程给出了一个非常简明的方法来判断某个元素该展现哪个 CSS 属性。

CSS 中，一个元素的 CSS 属性可能会有5种方式来决定，即 !important, inline, id, class, element。他们的优先级顺序也就是之前列出的顺序。但是对一个标签的选择并非是只有一种方式，比如可能出现 `.root #bone p.leaf` 这种选择方式，于是我们就引入了“**优先级表**”。

一个优先级表可以表述为“0 0 0 0”的形式，第一位代表 inline 的个数，第二位代表 id 的个数，第一位代表 class 的个数，第一位代表 element 的个数，之前 `.root #bone p.leaf` 就可以表述为"0 1 2 1"，即有1个 id 选择、2个 class 选择、1个 element 选择。对比每个 CSS 描述的优先级表，就可以知道某个标签所要表述的属性是什么了。

对于优先级表相同的 CSS 描述，则是后出现的描述的属性会覆盖之前出现的。