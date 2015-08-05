---
layout: post
title: "Package Adobe Air Application"
description: ""
category: Adobe Air
tags: [javascript, Adobe Air]

---

最近在做一个Air程序，研究了一下Air的打包过程，现总结如下，方法使用于Flex &amp; Ajax。

####打一个Air包

---

首先，是打包成最原始的Air包，这个在<a href="http://help.adobe.com/en_US/air/build/WS5b3ccc516d4fbf351e63e3d118666ade46-7ecc.html#WS5b3ccc516d4fbf351e63e3d118666ade46-7fa9">Air的官网就有讲</a>，我在这里摘录一下。
当你的程序能够成功运行时，你可以使用ADT工具来将其打包成一个AIR安装文件。一个AIR安装文件是一个压缩文件，它包含了你要发布给用户的所有程序文件。当然，当你安装这个AIR安装文件之前，你要先安装Adobe AIR。

为了确保程序的安全性，所有的AIR安装文件都<strong>必须</strong>有数字签名。不过，当开发时，你可以自己生成一个基本的自签名证书。证书可以用ADT工具或者是其他生成工具生成。你也可以从类似VeriSign或者Thawte这些认证结构购买一个商业的代码签名证书。当用户安装一个自签名的AIR文件时，在安装过程中，“发布商”一项会被显示成“unknown”。这是因为自签名的证书只能标识“该AIR程序自发布后没有被更改过”。这不能组织有人把一个伪造的文件进行自签名来伪装成你的AIR文件。对那些要公开发布的AIR文件而言，一个可以验证的商业证书是非常必要的。

那么，首先是要生成一个自签名的安全证书

    adt -certificate -cn SelfSigned 1024-RSA sampleCert.pfx samplePassword
    
接下来，就是创建一个安装文件了(在同一行执行)

    adt -package -storetype pkcs12 -keystore sampleCert.pfx HelloWorld.air     
    HelloWorld-app.xml HelloWorld.html AIRAliases.js


####打一个native包（native exe）

---

打这样一个包也是在命令行中执行，这样的包个人感觉没有什么大的作用，因为依然要让用户实现安装一个Adobe AIR，生成的exe文件直接在没有AIR环境的机器上是不能执行的。这个方式在<a href="http://help.adobe.com/en_US/air/build/WS789ea67d3e73a8b22388411123785d839c-8000.html">官网有讲</a>，摘录如下：
下面的代码创建了一个exe文件（一个Windows的本地安装文件，在同一行执行）：

    adt -package -storetype pkcs12 -keystore myCert.pfx 
        -target native 
        myApp.exe 
        application.xml 
        index.html resources

####打一个有运行时环境的包（bundle）

---

打一个有运行时环境的包，意思就是说，把AIR运行时放在程序目录中，程序运行就会调用程序目录中的AIR运行时来解释程序。使用ADT工具打包，打包出来是一个文件夹，文件夹中就是发布程序所需要的东西（程序和运行时）。接下来就是要使用任意你所喜欢的打包工具将文件夹再次打包。

可以想见，这样会有一些问题：

A. 我们不能通过AIR的升级系统来升级，即不能升级AIR运行时环境，也不能升级我们的程序，所以需要自己来做升级机制。那么可以预见到的需要解决的问题有这些：版本控制，修改安装目录的文件。

B. 不能通过Flash的唤起机制（LocalConnection, launchApplication）来启动客户端。于是可以预见到的需要解决的问题有这些：注册表的修改，mimetype，Register protocol。

[官网](http://help.adobe.com/en_US/air/build/WSfffb011ac560372f709e16db131e43659b9-8000.html)有讲，还列出了其他的缺点。打包代码摘录如下：

用ADT _bundle_ target来打包：
   
    adt -package  -keystore ..\cert.p12 -storetype pkcs12 
        -target bundle 
        myApp 
        myApp-app.xml 
        myApp.swf icons resources


####打一个同时安装AIR环境和程序的包

---

个人感觉，这样的包是最合适的，即能够将AIR和程序一起发布，同时安装，减少用户的操作负担，又能够使用AIR的所有特性，实在是一举多得。

* 首先要下载 <a href="http://airdownload.adobe.com/air/distribution/latest/win/AIR_Win_installer_files.zip" target="_blank">AIR_Win_installer_files.zip</a>，你也可以获取<a href="http://www.adobe.com/products/air/runtime-distribution3.html" target="_blank">其他系统的安装文件</a>。
* 解压AIR_Win_installer_files.zip，在解压出的目录中新建文件airinstall.cfg，在airinstall.cfg中添加你的air程序名称。将Air程序拷贝到该目录下。
* 在命令行中，将该文件保存成".airinstall.cfg"，此时，在一台没有AIR运行时的环境中，运行Adobe AIR Installer.exe就可以看到，有一个灰色的必选的“安装Adobe Air”选项。 

    `copy airinstall.cfg .airinstall.cfg`
    
* 接下来，就是将目录中的所有文件，打包成一个自解压的exe文件，通过很多工具都可以达到目的，比如最常用的压缩工具winrar就可以完成这项工作。选择“自解压”、“静默解压”，将“解压完执行的程序”设置为 Adobe AIR Installer.exe 。压缩完毕之后，是一个exe文件，运行该文件，此时如果没有AIR运行时，会自动安装AIR，再安装程序。

