---
title: 'Webpack入坑笔记(一) 安装webpack和插件,执行编译任务'
tags: webpack js模块化
date: 2017-02-28T14:22:06.000Z
---

随着现在前端工程化技术的发展,各种打包工具和框架层出不穷,我这种老程序猿也要充充电,紧跟时代的发展,否则就要被淘汰了.废话不多说,先从webpack说起.

# 为什么选择Webpack

官网对webpack的定义是MODULE BUNDLER，他的目的就是把有依赖关系的各种文件打包成一系列的静态资源。 请看下图

# 安装

先安装好node和npm,因为webpack是一个基于node的项目,然后

```bash
cd <your project root> //选择你项目所在路径
npm init //初始化项目,如果你不愿意填写信息,直接一路回车即可
npm install -g webpack //全局安装webpack,这是为了使用webpack指令更方便,如果单纯的装着项目目录下,则在打webpack指令时需要补全路径
npm install webpack --dev-save 在项目的package.json中标明该项目依赖webpack
```

以上几步做完之后,你的文件夹下应该多出了package.json和node_modules目录

然后你需要新建一个webpack.config.js

webpack.config.js简单点来说就就是一个配置文件，所有的魔力都是在这一个文件中发生的。 这个配置文件主要分为四大块

- entry 入口文件 让webpack用哪个文件作为项目的入口
- output 出口 让webpack把处理完成的文件放在哪里
- module 模块 要用什么不同的模块来处理各种类型的文件
- plugins 插件 要使用哪些插件,插件的初始化及配置

我们还需要新建一个app目录来存放我们的开发源码

- /app

  - index.js
  - sub.js

以上完成后,我们的目录文件看起来应该是这样的

- /app

  - index.js
  - sub.js

- node_modules

- package.json
- webpack.config.js

接下来我们要在sub.js中编写一个hello world模块,并可以输出到index.js中。webpack原生支持AMD和CommonJS两种标准。也支持ES6，不过我们循序渐进，以后再研究ES6。

## sub.js

```javascript
//commonjs风格
function generateText(){
  var ele=document.createElement('h2');
  ele.innerHTML='Hello World H2'
  return ele;
}
module.exports=ele;
```

## index.js

然后在 index.js中引用这个模块

```javascript
var sub=require('./sub');
var app=document.createElement('div');
app.innerHTML='Hello World Index';
app.appendChild(sub());
document.body.appendChild(app);
```

代码写完了,完成了一个很简单的功能,输出一个module,并在index.js中引用他,最后会在页面里输出2个标题,但现在是没办法预览的,因为我们还没有把源码生成为浏览器可以辨识的格式,为了可以在浏览器中访问我们的页面,需要在webpack中对源码进行编译,然后输出.

# 配置Webpack

我们的目标是把这2个JS文件合并成一个文件,并输出到 /build/ 目录下,我们可以再build下新建一个index.html,再把合并后的js引用,但这样比较麻烦,所以我们这里引入第一个插件,可以自动快速的帮我们生成HTML.

```bash
npm install html-webpack-plugin --save-dev
```

安装好了插件,开始写webpack.config文件

```javascript
var path=require('path');
var HtmlwebpackPlugin=require('html-webpack-plugin');
//定义一些文件夹的路径
var ROOT_PATH=path.resolve(__dirname);
var APP_PATH=path.resolve(ROOT_PATH,'app');
var BUILD_PATH=path.resolve(ROOT_PATH,'build');

module.exports={
  entry:APP_PATH,
  output:{
    path:BUILD_PATH,
    filename:'bundle.js'
  },
  plguins:[
    new HtmlwebpackPlugin({
      title: 'Hello World app' //为新建的HTML页面添加Title
    })
  ]
}
```

然后在项目根目录运行

```bash
webpack
```

终端会显示一堆信息告诉你成功了,你会发现多出来一个build文件夹,点开里面的index.html会发现"helloword"已经插入到页面了.

上面的任务虽然完成了,但每次都要手动编译并预览页面在真正的开发状态下是不可行的,为此,最好新建一个开发服务器,可以检测我们的代码,当代码更新时自动编译并更新预览.

好在webpack已经有执行相关任务的插件webpack-dev-server,我们只需下载并配置即可

另外,每次要打开浏览器也很麻烦,最好可以一句命令自动打开浏览器,我们还需要安装插件open-browser-webpack-plugin, 在使用open-browser-webpack-plugin时,最新版本有bug会导致和webpack-dev-server冲突,热更新会失效,我们只使用0.0.2老版本

## 安装webpack-dev-server和open-browser-webpack-plugin

```bash
npm install webpack-dev-server --save-dev
npm install open-browser-webpack-plugin@0.0.2 --save-dev //指定版本安装插件
```

安装完毕后,在webpack.config配置(最新版本已经没有progress选项了)

```javascript
var webpack=require('webpack');
var path=require('path');//引用path模块
var HtmlWebpackPlugin=require('html-webpack-plugin');//引用自动生成html插件
var OpenbrowserPlugin = require('open-browser-webpack-plugin');//自动打开浏览器插件
//定义一些文件夹路径
var ROOT_PATH=path.resolve(__dirname);//__dirname是node.js的一个特殊变量,可以在任意模块内部用来获取当前模块文件所在目录的完整绝对路径
var APP_PATH=path.resolve(ROOT_PATH,'app');
var BUILD_PATH=path.resolve(ROOT_PATH,'build');

module.exports={
  //项目的文件夹 可以直接用文件夹名称,默认会找到index.js,也可以指定文件
  entry:APP_PATH,
  //输出的文件名,合并以后的JS会命名为bundle.js
  output:{
    path:BUILD_PATH,
    filename:'./bundle.js'
  },
  devServer:{
    historyApiFallback:true,
    hot:true,
    inline:true,
    port: 8080
  },
  //添加我们的插件,会自动生成一个html文件
  plugins:[
    new HtmlWebpackPlugin({title:'Hello World app --Title'}),
    new webpack.HotModuleReplacementPlugin(),
    new OpenbrowserPlugin({url:'http://localhost:8080',browser:"firefox",ignoreErrors:true})//该插件必须使用0.0.2版本,否则会导致无法热刷新
  ]
};
```

然后在package.json里配置一下运行命令,npm支持自定义命令

```javascript
...
"scripts": {
  "start": "webpack-dev-server --inline"
},
...
```

好了,现在所有的配置都好了,在项目根目录下输入 npm start,经过一段时间的编译,server已经启动,并自动打开了浏览器, 我们又见到了亲爱的"hello word",在JS里随便修改一些输出,然后保存,浏览器自动刷新,效果实现了!

[alt]: https://pic2.zhimg.com/55fb7d622403852ff7533c6da5c620cd_b.png

> 参考知识:https://zhuanlan.zhihu.com/p/20367175?columnSlug=FrontendMagazine
