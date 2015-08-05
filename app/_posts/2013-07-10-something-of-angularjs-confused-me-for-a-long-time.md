---
layout: post
title: "One thing of Angular.js that confused me"
description: ""
category: 
tags: [js, Angularjs]

---
{% include JB/setup %}


Angularjs 似乎是完全从另外的角度来做的架构，基于 MVC 的认知，加上易测试性的考虑，Angularjs 加入了大量的**设计模式**的东西，从流行的 js 开发模式中忽然进入这种世界，对于一个没有设计模式基础的人来说，理解许多东西都很费周章。虽然 Angularjs 的文档组织得很乱，读了几遍之后，我逐渐理解了其中一些让我困扰许久的问题。

官方文档的位置是：[http://docs.angularjs.org/guide/](http://docs.angularjs.org/guide/)

中文文档的位置是：[https://gitcafe.com/Angularjs/Angularjs-Developer-Guide](https://gitcafe.com/Angularjs/Angularjs-Developer-Guide)

### Dependency Injection (DI, 依赖注入)

依赖注入是一种设计模式，它还有一个名字叫做*控制反转*，可以用来减低计算机代码之间的耦合度。

从 官方文档：[Dependency Injection](http://docs.angularjs.org/guide/di) （[中文](https://gitcafe.com/Angularjs/Angularjs-Developer-Guide/blob/master/AngularJS%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%9714%EF%BC%9A%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5.md)）我们可以看到对依赖注入的简单讲解。根据我个人的理解，依赖注入就是这样（[看我这篇文章](http://witcher42.github.io/2013/07/10/dependency-injector-in-a-sentence/)）：

<pre><code class="java">
有一个类A，然后有个变量是 `private B b;`，这时 A 就“依赖”B。构造时就这么写 this.b = new B();
不过，如果把 b 当做参数传进来，就变成了依赖注入。 this.b = b;

</code></pre>


这样面临的问题是，传进来的 b 是要在实例化 A 的地方进行实例化的。所以有了全局的 `injector`，那它做了什么呢？那就是，我写了一个 A，A 依赖了一个 b，b 就给我传进来了。虽然 b 是别的地方定义或者实例化的，但是我不用关心这些。当别人依赖 a 的时候，也不用关心 a 从哪里来，只需要定义一个 C `function CCtrl(a) {}`， a 就这么传进来了。那么到底是什么时候实例化的呢？那是 injector 帮着做的。

这样做貌似感觉好多了，可是为什么要这么做呢？想来，好处似乎是这样的：

* 充分解耦
* 方便的单元测试

### 多样的编码风格

在学习 Controller 的时候，我见到了很多中书写风格。

比如

<pre><code class="javascript">
function LittleController($scope) {
  $scope.littleParams = 'littleValue';
}

</code></pre>
    
又比如

<pre><code class="javascript">
function LittleController(renamed$scope) {
  $scope.littleParams = 'littleValue';
}
LittleController.$inject = ['$scope'];
someModule.controller('LittleController', LittleController);

</code></pre>

再比如    

<pre><code class="javascript">
someModule.controller('LittleController', ['$scope', function(renamed$scope) {
    $scope.littleParams = 'littleValue';
}]);

</code></pre>

对于 Angularjs 是如何动态判断函数的第一个变量是 `$scope` 还是别的什么的，我推测是使用了类似 `LittleController.toString();` 的方式来检测函数的参数都是什么。第一种是最朴素的方式，不过对于需要压缩代码的项目来讲，就要悲剧了。所以有了第二种方式，通过 `$inject` 变量可以制定所依赖的结构是什么，也是文档中所推荐的方式。第三种方式，是为了避免 `$inject` 导致代码膨胀而出现的方式，不过个人觉得这种写法实在是丑爆了，而官方文档推荐第三种方式来书写各种工厂方法（ factory, directive, filter… ）。
    
具体看[这里（Inline Annotation）](http://docs.angularjs.org/guide/di#inlineannotation)、[这里（DI in controllers）](http://docs.angularjs.org/guide/di#diincontrollers)和[这里（Factory methods）](http://docs.angularjs.org/guide/di#factorymethods)。