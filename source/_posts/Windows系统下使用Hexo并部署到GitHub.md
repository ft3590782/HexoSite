---
title: Windows系统下使用Hexo并部署到GitHub
date: 2017-02-04 16:47:50
tags:
---
需用到的程序:

1.[Git for Windows](https://git-for-windows.github.io/)

2.[Node.js for Windows](https://nodejs.org/en/)


## 准备工作

首先安装好Git和Node.js,安装还是比较简单的不详细叙述了,需要注意的是安装Nodejs时最好选择最新版本的,因为集成了npm无需再自己安装了,比较方便,否则还需要自己安装Npm.
安装完成之后,点击开始菜单->运行CMD,打开CMD检查一下git,nodejs,npm是否已经安装成功,检查命令如下:

git
输入
``` bash
git --version
```
成功的话会看到类似如下信息:
``` bash
git --version
git version 2.10.0.windows.1
```

nodejs
输入
``` bash
node -v
```
成功的话会看到类似如下的版本号:
``` bash
node -v
v6.2.0
```

npm
输入
``` bash
npm -v
```
成功的话会看到类似如下版本号:
``` bash
npm -v
3.8.9
```

如果以上3个命令都能显示预期结果,说明我们的开发环境准备完成了,可以进入下一步开始安装Hexo了.

## 安装Hexo

依次运行以下命令

全局安装hexo手脚架
``` bash
npm install hexo-cli -g
```

安装到指定路径,dir是你需要安装到的路径和项目名称
``` bash
hexo init <dir>
```
例如:我需要安装到D盘下的Project文件夹下,项目文件夹叫HexoBlog,则该命令应该是:
``` bash
hexo init d:/Project/HexoBlog
```
跳转到指定路径下
``` bash
cd <dir>
```
跳转到刚才安装的目录,例如我们这里应该是:
``` bash
d:
cd d:/Project/HexoBlog
```
安装依赖的modules,我这里在安装手脚架时自动安装了所有node_modules,如果你不确定你是否完整安装了所有开发所需modules,可以手动再运行一次.
``` bash
npm install
```
运行一个简易的本地服务器预览生成的页面,默认的地址是http://localhost:4000/
``` bash
hexo server
```
到这里位置,Hexo已经安装好了

## 添加文章

在cmd中,指向当前目录的情况下,可以通过以下命令来新建帖子或页面:

``` bash
hexo new [layout] <title>
```

比如以下命令会新建一篇标题为"HelloWorld"的帖子,因为默认的layout为post类型:

``` bash
hexo new HelloWorld
```

## 生成和发布

生成的命令:
``` bash
hexo g or hexo generate
```

发布
首先把你的hexo项目 commit到你的github pages项目下,然后有两种方法可以发布""
``` bash
hexo g -d 表示生成后立即发布
hexo d 直接发布之前通过hexo g生成的项目
```

如果你需要添加自己的域名,在/source/目录下新建一个CNAME文件,里面放入你的域名
例如:
``` bash
blog.cloudjay.net
```
关于github pages的具体说明,具体可以参考github pages的官方文档:[Github Pages Help](https://help.github.com/categories/github-pages-basics/)
