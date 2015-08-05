---
layout: post
title: "Exit Status of Grep in While in shell command"
description: ""
category: Shell
tags: [shell, grep, exit status]
---
{% include JB/setup %}

首先看一段代码：
    
    if grep nosuchpattern /etc/passwd ; then
        echo "I found nosuchpattern in file /etc/passwd"
    else
        echo "I did not find nosuchpattern in file /etc/passwd"
    fi

上面的判断会进入哪个分支？当然是第二个，这是理所当然的。不过细思恐极，正常 Shell 的 exit status 不应该是在成功的时候返回 0、失败的时候返回 1 么？这样看来应该是进入另一个分支啊，但是从语意上这么整就不对。那么事情的真相只能是当 exit status 为 0 的时候进入 success 分支，事实也是如此，*Command Shell Language* 摘录如下：

> #### The while Loop
> 
> The while loop shall continuously execute one compound-list as long as another compound-list has a zero exit status.
> 
> The format of the while loop is as follows:
> 
>     while compound-list-1do
>         compound-list-2
> 
>     done
> 
> The compound-list-1 shall be executed, and if it has a non-zero exit status, the while command shall complete. Otherwise, the compound-list-2 shall be executed, and the process shall repeat.
 
还有
 
> #### The if Conditional Construct
> 
> The if command shall execute a compound-list and use its exit status to determine whether to execute another compound-list.
> 
> The format for the if construct is as follows:
> 
>     if compound-listthen
>         compound-list[elif compound-listthen
>         compound-list] ...
>     [else
>         compound-list]
> 
>     fi
> 
> The if compound-list shall be executed; if its exit status is zero, the then compound-list shall be executed and the command shall complete. Otherwise, each elif compound-list shall be executed, in turn, and if its exit status is zero, the then compound-list shall be executed and the command shall complete. Otherwise, the else compound-list shall be executed.

也就是说，如果 if 的条件的 exit status 是 zero，则会进入 success 分支。