---
layout: post
title: "Launch node-webkit as admin"
description: ""
category: NodeWebkit
tags: [javascript, node-webkit, admin, trustInfo, requireAdministrator]
---
{% include JB/setup %}

正在考虑 [Switcher](https://github.com/Witcher42/Switcher) 的 Windows 版本，遇到一个问题，Switcher 需要操纵 hosts 文件，而正常情况下它是没有权限的，于是问题就是「如何让程序一直以管理员的身份运行」？


这个问题在 Windows 上的表现就是怎么让程序开始运行的时候弹出「UAC」对话框，它是由程序的 manifest 文件来控制的：[http://msdn.microsoft.com/en-us/library/bb384691.aspx](http://msdn.microsoft.com/en-us/library/bb384691.aspx)。

简单来说，改变一个程序的 UAC 状态，就需要改变它的 manifest 中的相关配置：

    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    	<trustInfo xmlns="urn:schemas-microsoft-com:asm.v3">
    		<security>
    			<requestedPrivileges>
    				<requestedExecutionLevel level="requireAdministrator" uiAccess="false"></requestedExecutionLevel>
    			</requestedPrivileges>
    		</security>
    	</trustInfo>
    </assembly>

注意 `requestedExecutionLevel` 这一项，表示的就是执行时候需要的级别，可以指定的级别有下面三种，而需要 Admin 权限的话，只需要指定 *level* 为 *requireAdministrator*。

    <requestedExecutionLevel  level="asInvoker" uiAccess="false" />
    <requestedExecutionLevel  level="requireAdministrator" uiAccess="false" />
    <requestedExecutionLevel  level="highestAvailable" uiAccess="false" />

接下来就是修改 Switcher 的 manifest 了,我最开始时是推测了三种方式，最终发现两种可行：

* （不可行）对最终构建出的 exe 文件进行修改
* （可行）用 C# 或是 C/C++ 写一个需要 Admin 权限的引导程序来启动 Switcher.exe
* （可行）对 nw.exe 的 manifest 进行修改，再进行构建

我这里只说一下第三种，第二种有很大的问题：一是多出了一个 exe，一是 C# 需要装平台，C/C++ 又要考虑太多兼容性，再就是构建的时候还要跑到 Windows 上执行。而第三种方案没有上面的问题，只需要对 nw.exe 做一次修改，就可以了。

* 修改 nw.exe 的 manifest，这里是使用 mt.exe 来修改：[http://msdn.microsoft.com/en-us/library/ms235591.aspx](http://msdn.microsoft.com/en-us/library/ms235591.aspx)

      mt.exe –manifest nw.exe.manifest -outputresource:nw.exe;1

    其中 mt.exe 可以通过安装 Visual Studio 来获得，安装之后的路径在类似这样的地方
    
      C:\Program Files (x86)\Microsoft SDKs\Windows\v7.1A\Bin\x64\mt.exe
      
    其中 manifest 可以从 nw.exe 中得到
    
      mt.exe -inputresource:nw.exe;#1 -out:nw.exe.manifest
      
    得到的 nw.exe.manifest 中的 level 是 asInvoker，需要将其修改为 requireAdministrator。修改之后通过上面的命令就可以修改 nw.exe 的 manifest 了。
    
* 之后就是构建自己的程序 Switcher.exe 了，我使用的是「[grunt-node-webkit-builder](https://github.com/mllrsohn/grunt-node-webkit-builder)」，具体的方式可以参考它的文档：[https://github.com/mllrsohn/grunt-node-webkit-builder#grunt-node-webkit-builder](https://github.com/mllrsohn/grunt-node-webkit-builder#grunt-node-webkit-builder)。