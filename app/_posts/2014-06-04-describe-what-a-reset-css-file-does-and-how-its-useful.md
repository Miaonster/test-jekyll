---
layout: post
title: "Reset CSS 文件的作用和使用它的好处"
description: ""
category: 
tags: [Front-end-Developer-Interview-Questions, css]

---
{% include JB/setup %}

Reset CSS（CSS 重置），作用是让各个浏览器的 CSS 的样式有一个统一的基准。

比较流行的 CSS Reset 有「Eric Meyer’s Reset CSS」、「Normalize.css」、「YUI Reset」等等，进一步可以看「[CSS Reset](http://www.cssreset.com/)」。

现在有一些观点是不要去做 Reset，或许会给以后的 CSS 框架的编写提供一个思路。一个是说没有必要再去做 Reset CSS，因为既然之后要再用新的样式覆盖掉，为什么不直接写新的样式呢，直接写一个 global.css 就好了。再有就是根本不需要特殊的 Reset，使用 `* { margin: 0; padding: 0; }` 就 OK 了，因为并没有关于这种写法会造成性能低下的反馈，而通配符的存在就是要用的。