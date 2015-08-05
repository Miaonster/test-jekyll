---
layout: post
title: "slow innerHTML"
description: ""
category: 
tags: [js, html]

---
{% include JB/setup %}

最近在找程序的系能瓶颈，发现了一个有趣的地方，是关于 innerHTML 的，比如我想删除一个 node 下的所有节点的话，大致有两种方式。

第一种
    
    node.innerHTML = '';
    
第二种
    
    while (node.firstChild) {
        node.removeChild(node.firstChild);
    }
    
我在程序中使用的是第一种方式，之前的印象中，不知什么时候有了“操作 `innerHTML` 比较快”这样的观点。然而，在 Adobe Air 环境下，我发现这里不仅速度非常慢，而且有大量的内存泄露。为此，查找了相关资料，发现通过对比这两种方式，`innerHTML` 比 `while` 慢了三百多倍，这实在是让人惊讶。

具体的比较看这里：[http://jsperf.com/innerhtml-vs-removechild](http://jsperf.com/innerhtml-vs-removechild)。

不过这个仔细想来也是题中应有之义，因为每次的 innerHTML 都会导致对 html 的重新解析和渲染，这肯定是非常慢的。