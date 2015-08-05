---
layout: post
title: "XHGui quick overview"
description: ""
category: 
tags: [php, xhgui, xhprof, mongodb]
---
{% include JB/setup %}

> A graphical interface for XHProf data built on MongoDB

基于 MongoDB 的 XHProf 图形界面。

### Basic

首先，我们确保已经知道了什么是 XHProf，不知道的话请戳 [XHProf quick overview](http://witcher42.github.io/2013/12/24/xhprof-quick-overview/)。

然后来说一说 XHGui，它基于 XHProf 和 MongoDB，将 XHProf 的内容存储到 Mongo 中，利用自己的系统来解析和展示。

### Installation

#### Requirements

先安装依赖

* XHProf

    [XHProf quick overview install](http://witcher42.github.io/2013/12/24/xhprof-quick-overview/#install) 中有提到。

* MongoDB PHP

        sudo pecl install mongo

* MongoDB Server（如何需要的话）

    看官方的文档，如果你有一个 MongoDB 的话，就不用装了。

* mcrypt

    Mac 下用 brew 安装会这个比较难过，Debian-based Linux 的话，可以直接
    
        sudo apt-get install php5-mcrypt
        
之后将 php.ini 的 extension 都写好，重启 php-fpm。

#### Installing Xhgui

接下来就是安装 XHGui 了，Github 上的 README 给出了详细的安装步骤：[https://github.com/perftools/xhgui?source=c#installation](https://github.com/perftools/xhgui?source=c#installation)。

需要说明的是

* XHGui 是用的 Composer 来安装的依赖，国内可能会比较慢。

* 在 config/config.php 中设置 Mongo 的 username 和 password 用的是 PHP MongoClient 的 $server 的形式

      mongodb://[username:password@]host1[:port1][,host2[:port2:],...]/db

### Configuration

查看 README.md [https://github.com/perftools/xhgui?source=c#configure-webserver-re-write-rules](https://github.com/perftools/xhgui?source=c#configure-webserver-re-write-rules)

### Usage

    <?php
    require_once $PATH_TO_XHGUI . '/external/header.php';
    
然后就可以 webroot/index.html 来访问具体的结果了。


![](/assets/images/xhprof-quick-overview-5.png)

### Digression

使用时发现了一个问题，这里的 Callgraph 难用到爆，虽然看上去很绚，但是完全不实用，没有 XHProf 原生的那样清晰，试着将原来的功能引入，也不算太复杂，只是有些繁复。