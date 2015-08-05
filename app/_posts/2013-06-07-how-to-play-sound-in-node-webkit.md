---
layout: post
title: "How to play sound in node webkit"
description: ""
category: javascript
tags: [javascript, node, node-webkit]

---
{% include JB/setup %}

For [license reason](https://github.com/rogerwang/node-webkit/wiki/Support-mp3-and-h264-in-video-and-audio-tag), [node-webkit](https://github.com/rogerwang/node-webkit) cannot play mp3, but we can do it in other ways.

The wiki gives us two methods to do that.

1. Copy the _copy those media codec files from Chrome_.
2. Just _Build your own_ node-webkit.

The first one doesn't work at all for me right now. And the second one is too complex for huge compiling a C++ project is a miserable thing as I learnt several years ago.

So I find a solution after thinking for a while.

Include a third party node-module which can play a sound, like _[play](https://npmjs.org/package/play)_. Then use the api the node-module provide to play sound.