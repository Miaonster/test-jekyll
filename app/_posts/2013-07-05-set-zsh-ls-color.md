---
layout: post
title: "Set ls color on zsh"
description: ""
category: 
tags: []
---

{% include JB/setup %}

ZSH 的一个配置项 [oh-my-sh](https://github.com/robbyrussell/oh-my-zsh) 是一个很厉害的开源项目，有非常强大的插件系统和主题系统，最近果断由 bash 切换到 zsh。不过切换了之后发现 ls 的时候，颜色有点诡异，于是简单修改了一下颜色。zsh 中，ls 的 color 是由 LSCOLORS 来控制的，只需要改变它的颜色就可以了。

LSCOLORS 看起来是这样的 "exfxcxdxFxcgcdcbcgeced"，其中每两位表示一个 attribute 的“前景色”和“背景色”，比如前两个字母“ex”表示的就是：文件夹的前景色是 e(blue)，而背景色是 x(default)。

具体的颜色指示器如下：

     a     black
     b     red
     c     green
     d     brown
     e     blue
     f     magenta
     g     cyan
     h     light grey
     A     bold black, usually shows up as dark grey
     B     bold red
     C     bold green
     D     bold brown, usually shows up as yellow
     E     bold blue
     F     bold magenta
     G     bold cyan
     H     bold light grey; looks like bright white
     x     default foreground or background

LSCOLORS 中 attribute 的顺序如下：

    1.   directory
    2.   symbolic link
    3.   socket
    4.   pipe
    5.   executable
    6.   block special
    7.   character special
    8.   executable with setuid bit set
    9.   executable with setgid bit set
    10.  directory writable to others, with sticky bit
    11.  directory writable to others, without sticky bit
    
其实，只需要 `man ls` 就可以看到对 LSCOLORS 的描述了。