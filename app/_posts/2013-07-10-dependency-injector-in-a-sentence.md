---
layout: post
title: "Understand Dependency Injector"
description: ""
category: 
tags: [心有灵犀一点通]

---
{% include JB/setup %}

有一个类A，然后有个变量是 `private B b;`，这时 A 就“依赖”于 B，构造时就这么写

<pre>
<code class="java">
this.b = new B();
</code>
</pre>

不过，如果把 b 当做参数传进来，就变成了依赖注入。 

<pre>
<code class="java">
this.b = b;
</code>
</pre>

这么做的道理似乎是为了“方便单元测试”？