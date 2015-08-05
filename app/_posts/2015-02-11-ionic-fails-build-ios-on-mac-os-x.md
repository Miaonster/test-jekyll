---
layout: post
title: "Ionic Fails Build iOS on Mac OS X"
description: ""
category: 
tags: [ionic, hybrid app, cordova, grep, build, ios, mac, mac os x]

---
{% include JB/setup %}

Recently, I'm learning [ionic](http://ionicframework.com/getting-started/), It's an amazing project, whit which you can *build mobile apps faster with the web technologies you know and love*.

But I met with a strange problem. I cannot even pass the steps of *Getting Started*. Let me relate how it happened and what the reason was.

### Problem

The first step is to install ionic and cordova, it's easy.


    $ npm install -g cordova ionic


Then I started a project, *myApp*, successfully.


    $ ionic start myApp tabs


The problem occured in this third step. It throwed an exception when I ran command below:


    $ cd myApp
    $ ionic platform add ios
    $ ionic build ios


And the exception is:


    xcodebuild: error: 'myApp.xcodeproj.xcodeproj' does not exist.


### Reason

There're two `.xcodeproj`. It's strange. So I scanned code in the build shell script.


    /myApp/platforms/ios/cordova/build


It seemed all the things are usual. But I noticed `basename` command doesn't work as expected. 

In this line


    PROJECT_NAME=$(basename "$XCODEPROJ" .xcodeproj)


`basename` command doesn't remove `"$XCODEPROJ`'s suffix, `.xcodeproj`. What's more, `.xcodeproj` is displayed in another style (red color in my theme).

Therefore I realzed that the reason is the color style of `"$XCODEPROJ`. After checking codes above, I got the point.

It's **`grep`** in this line:


    XCODEPROJ=$( ls "$PROJECT_PATH" | grep .xcodeproj  )


I'm using zsh and on-my-zsh, and `grep` has an option named `GREP_OPTIONS`. It's always defined in `~/.zshrc`:


    export GREP_OPTIONS='--color=auto'


So the solution is just that easy and obvious:

### Solution

Remove `GREP_OPTIONS` in `~/.zshrc`.

### What's more

Some questions still remain.

* Why settings in `~/.zshrc` effect when build shell script uses `#!/bin/bash`?
* Why don't settings in *oh-my-zsh* effect?