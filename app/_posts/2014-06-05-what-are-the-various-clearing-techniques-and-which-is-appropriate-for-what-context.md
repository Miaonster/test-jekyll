---
layout: post
title: "清除浮动及其使用场景"
description: ""
category: 
tags: [Front-end-Developer-Interview-Questions, html]

---
{% include JB/setup %}

#### Empty div

    <div style="clear: both;"></div>
    
空的 div 就是一个空的 div，或者也可以用其他的元素，比如 `<br/>`。但是呢，如果你追求语义化的话，肯定不会使用这种方式的，因为，它就是一个 div，不表示的任何东西，也就是在语义上没有任何意义。

#### Pseudo selector

    .container:before,
    .container:after {
      content:"";
      display:table;
    }
    .container:after {
      clear:both;
    }
    .container {
      *zoom:1; /* For IE 6/7 (trigger hasLayout) */
    }
        
目前使用比较广泛，效果很不错的 clearfix 方案。

#### 题外话

关于 Clearfix 的讨论以及很久了，float 的很大的一个应用场景就是在布局上，现在出现的新标准正在逐步让 float 回归到自己的本来角色中。`display: inline-block;` 可以实现原来很大程度上使用 float 的目的，还有 flexbox 则完全是为了自适应的布局来设计的，有了 flexbox 基本可以省掉 float 这个 trick 了（虽然还有性能的问题）。