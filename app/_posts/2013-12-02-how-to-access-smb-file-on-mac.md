---
layout: post
title: "How to access smb files on Mac OSX"
description: ""
category: 
tags: [alfred, smb]
---
{% include JB/setup %}

在 windows 上分享文件的时候，有时会用到双反斜杠打头的路径，这种路径是一种叫做 *SMB* 的协议，在 Mac 上的写法是 smb 打头的，具体的例子：

    # 反双斜杠打头的路径
    \\domain\path\file
    
    # smb 打头的路径
    smb://domain/path/file

### What's SMB

戳这里「[SMB 服务器信息区块](http://zh.wikipedia.org/wiki/%E4%BC%BA%E6%9C%8D%E5%99%A8%E8%A8%8A%E6%81%AF%E5%8D%80%E5%A1%8A)」。

### How to access

在 Mac 下怎么访问这个路径呢？首先要将 windows 上的路径格式转换为 `smb://...` 的格式，例如上面的地址需要转换为这样 `smb://domain/path/file`。接下来，执行这些步骤：

* 打开 finder
* 快捷键 Cmd + K
* 输入正确的路径来进行访问

### Alfred workflow

另外，我写了一个简单的 Alfred workflow，这个 workflow 的作用就是可以在两种路径格式中互相转换，请戳「[Alfred SMB](https://github.com/Witcher42/Alfred-smb)」。

