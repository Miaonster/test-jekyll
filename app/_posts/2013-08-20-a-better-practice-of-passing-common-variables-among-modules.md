---
layout: post
title: "Best Practice - How to pass common object among modules"
description: ""
category: 
tags: [js]
 
---
{% include JB/setup %}

在模块化编程中一直困扰我的一个问题是，如何在各个 modules 中传递共有的一些结构。我一直都是将这些结构变成全局变量`global`下的一个子结构，最近在思索编码实践之类的问题，想到了这个，在 stackoverflow 上查到了[这个](http://stackoverflow.com/questions/10306185/nodejs-best-way-to-pass-common-variables-into-separate-modules)。

答案说

>  I have found using dependency injection, to pass things in, to be the best style.

就是说“依赖注入”或者叫“控制反转”是一个不错的方式，我忽然发觉 AnjularJs 中的控制翻转用得真是很精妙，完全避免了全局变量的问题。

代码如下：

    // App.js
    module.exports = function App() {
    };
    
    // Database.js
    module.exports = function Database(configuration) {
    };
    
    // Routes.js
    module.exports = function Routes(app, database) {
    };
    
    // server.js: composition root
    var App = require("./App");
    var Database = require("./Database");
    var Routes = require("./Routes");
    var dbConfig = require("./dbconfig.json");
    
    var app = new App();
    var database = new Database(dbConfig);
    var routes = new Routes(app, database);
    
    // Use routes.