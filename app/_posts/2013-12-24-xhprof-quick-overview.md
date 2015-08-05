---
layout: post
title: "XHProf quick overview"
description: ""
category: 
tags: [php, xhprof]
---
{% include JB/setup %}

> XHProf is a function-level hierarchical profiler for PHP and has a simple HTML based user interface.

XHProf 是由 Facebook 开发的性能分析工具：[https://github.com/facebook/xhprof](https://github.com/facebook/xhprof)。

### Basic

我们可以在 [http://php.net/xhprof](http://php.net/xhprof) 找到一些文档，是关于安装和函数相关的介绍。至于更加丰富的文档，在源码的 [xhprof_html/docs/index.html](https://github.com/facebook/xhprof/blob/master/xhprof_html/docs/index.html) 文件中，我们需要将源码 clone 的本地之后再去查看。且摘录这份文档的目录来看一下：

     1. Introduction
     2. XHProf Overview
     3. Installing the XHProf extension
     4. Profiling using XHProf
     5. Setting up the XHProf UI
     6. Notes on using XHProf in production
     7. Lightweight Sampling Mode
     8. Additional features
     9. Dependencies
    10. Acknowledgements

另外有几点需要需要说明：

* XHProf 本身有个很简陋的 UI 系统，[xhprof_html/docs/index.html](https://github.com/facebook/xhprof/blob/master/xhprof_html/docs/index.html) 的第五章「Setting up XHProf UI」可以看到详细介绍，下文也会重点介绍一番。
    
* 还有，XHProf 有一个的分支，它的主要目标是将数据存储在 Mysql 数据库中使用，是由「Paul Reinheimer」维护的 [XHProf UI](https://github.com/preinheimer/xhprof)，不过这个已经废弃了，Reinheimer 的主页上明确说它 Deprecated：[http://blog.preinheimer.com/about.html](http://blog.preinheimer.com/about.html)。

* 再有，目前看上去不错的 UI 有 [XHGui](https://github.com/perftools/xhgui) 和 [xhprof.io](http://xhprof.io/)。

### Install

基本信息介绍完了，下面来说下 XHProf 的安装，很简单：

    sudo pecl install xhprof
    
当然，它会告诉你现在还不是 stable 版本

> Failed to download pecl/xhprof within preferred state "stable", latest release is version 0.9.4, stability "beta", use "channel://pecl.php.net/xhprof-0.9.4" to install

所以目前的安装方式是：

    sudo pecl install channel://pecl.php.net/xhprof-0.9.4
    
安装完毕之后在 php.ini 中加入响应配置：

    extension=xhprof.so
    
重启 php-fpm。

### Usage

闲言少叙，废话休提，说一说如何使用。

#### Simplest Usage

安装完毕之后，我们可以开始使用 XHProf 了，比较官方的简单 demo 如下：

    <?php
    
    // start profiling
    xhprof_enable();
    
    // call some functions
    foo();
    
    // stop profiler
    $xhprof_data = xhprof_disable();
    
    // display raw xhprof data for the profiler run
    print_r($xhprof_data);


#### Data Structure

于是就得到了 `$xhprof_data`，数据结构类似于：

    Array
    (
        [main()==>foo] => Array
            (
                [ct] => 1
                [wt] => 21
            )
    )


返回的结构是一个 array，里面的每一项表示的是某个函数的调用次序，比如第一个是 `main()==>foo` 表示的是 `main` 到 `foo` 的调用，其中字段的具体含义

* ct 是 cpu time
* wt 是 wall time。

更详细的资料见 [xhprof_html/docs/index.html](https://github.com/facebook/xhprof/blob/master/xhprof_html/docs/index.html) 。

### XHProf UI

如上文所说，XHProf 有很简单的 UI 系统，就像这样。

![xhprof ui](/assets/images/xhprof-quick-overview-1.png)

而关于 UI，不得不再次提到 [xhprof_html/docs/index.html](https://github.com/facebook/xhprof/blob/master/xhprof_html/docs/index.html)，文章中详细提及了 XHProf UI 的配置和使用方式，下面简要说一下重点内容：

#### PHP source structure

UI 部分由 PHP 实现，代码都在 xhprof_html/ 和 xhprof_lib/ 中。

其中 xhprof_html 有三个顶级文件

* index.php: For viewing a single run or diff report.
* callgraph.php: For viewing a callgraph of a XHProf run as an image.
* typeahead.php: Used implicitly for the function typeahead form on a XHProf report.

而 xhprof_lib 中的代码为分析和显示提供支持。

#### Access

访问 index.php 可以看到上图中的页面。

#### Managing XHProf Runs

在 [xhprof_html/docs/index.html](https://github.com/facebook/xhprof/blob/master/xhprof_html/docs/index.html) 中介绍了如何才能将使用这个 UI，将代码摘录在下面，其中的主要思路就是在 `$xhprof_data` 得到之后将起存起来：


    <?php
    // start profiling
    xhprof_enable();
    
    // run program
    ....
    
    // stop profiler
    $xhprof_data = xhprof_disable();
    
    //
    // Saving the XHProf run
    // using the default implementation of iXHProfRuns.
    //
    include_once $XHPROF_ROOT . "/xhprof_lib/utils/xhprof_lib.php";
    include_once $XHPROF_ROOT . "/xhprof_lib/utils/xhprof_runs.php";
    
    $xhprof_runs = new XHProfRuns_Default();
    
    // Save the run under a namespace "xhprof_foo".
    //
    // **NOTE**:
    // By default save_run() will automatically generate a unique
    // run id for you. [You can override that behavior by passing
    // a run id (optional arg) to the save_run() method instead.]
    //
    $run_id = $xhprof_runs->save_run($xhprof_data, "xhprof_foo");
    
    echo "---------------\n".
         "Assuming you have set up the http based UI for \n".
         "XHProf at some address, you can view run at \n".
         "http://<xhprof-ui-address>/index.php?run=$run_id&source=xhprof_foo\n".
         "---------------\n";


更多东西可以去看 [xhprof_html/docs/index.html](https://github.com/facebook/xhprof/blob/master/xhprof_html/docs/index.html)。

#### Callgraph

什么是 callgraph 呢？它是一张图，是通过上面说的 `$xhprof_data` 所包含的调用次序来生成的，类似于下面这种，我们可以很容易地看到耗时最长的一条调用路径，非常直观。我们通过上面那张截图中的连接「View Full Callgraph」就可以访问到。

![xhprof ui](/assets/images/xhprof-quick-overview-2.png)

### End

XHProf 的介绍已经结束了，那么如何将其应用的生产环境呢？有这么几个思路：

* 直接访问 index.php 使用简单的 UI 系统来查看

![](/assets/images/xhprof-quick-overview-4.png)

* 使用 XHProf UI 分支，这个要更丰富一些

![](/assets/images/xhprof-quick-overview-3.png)

* 使用 [XHGui](https://github.com/perftools/xhgui) 或是 [xhprof.io](http://xhprof.io/)，我只使用了 XHGui，所以只给出它的介绍，请戳 [XHGui Quick Overview]()。

![](/assets/images/xhprof-quick-overview-5.png)
