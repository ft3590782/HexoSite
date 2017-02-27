---
title: Webpack入坑笔记(一)
tags:
---
随着现在前端工程化技术的发展,各种打包工具和框架层出不穷,我这种老程序猿也要充充电,紧跟时代的发展,否则就要被淘汰了.废话不多说,先从webpack说起.

##为什么选择Webpack

官网对webpack的定义是MODULE BUNDLER，他的目的就是把有依赖关系的各种文件打包成一系列的静态资源。 请看下图

[alt]: https://pic2.zhimg.com/55fb7d622403852ff7533c6da5c620cd_b.png

##安装

先安装好node和npm,因为webpack是一个基于node的项目,然后

``` bash
cd <your project root> //选择你项目所在路径
npm init //初始化项目,如果你不愿意填写信息,直接一路回车即可
npm install -g webpack //全局安装webpack,这是为了使用webpack指令更方便,如果单纯的装着项目目录下,则在打webpack指令时需要补全路径
npm install webpack --dev-save 在项目的package.json中标明该项目依赖webpack
```

以上几步做完之后,你的文件夹下应该多出了package.json和node_modules目录

然后你需要新建一个webpack.config.js

webpack.config.js简单点来说就就是一个配置文件，所有的魔力都是在这一个文件中发生的。 这个配置文件主要分为四大块

* entry 入口文件 让webpack用哪个文件作为项目的入口
* output 出口 让webpack把处理完成的文件放在哪里
* module 模块 要用什么不同的模块来处理各种类型的文件
* plugins 插件 要使用哪些插件,插件的初始化及配置
