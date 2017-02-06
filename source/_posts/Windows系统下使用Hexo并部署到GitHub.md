---
title: Windows系统下使用Hexo并部署到GitHub
date: 2017-02-04 16:47:50
tags:
---
# Windows系统下使用Hexo并部署到GitHub
需用到的程序:
1.[Git for Windows](https://git-for-windows.github.io/)
2.[Node.js for Windows](https://nodejs.org/en/)


## 准备工作

首先安装好Git和Node.js,安装还是比较简单的不详细叙述了,需要注意的是安装Nodejs时最好选择最新版本的,因为集成了npm无需再自己安装了,比较方便,否则还需要自己安装Npm.
安装完成之后,点击开始菜单->运行CMD,打开CMD检查一下git,nodejs,npm是否已经安装成功,检查命令如下:

git
输入```git --version```
成功的话会看到类似如下信息:
```
git --version
git version 2.10.0.windows.1```

nodejs
输入```node -v```
成功的话会看到类似如下的版本号:
```
node -v
v6.2.0```

npm
输入```npm -v```
成功的话会看到类似如下版本号:
```
npm -v
3.8.9```

如果以上3个命令都能显示预期结果,说明我们的开发环境准备完成了,可以进入下一步开始安装Hexo了.

## 安装Hexo

依次运行以下命令

```npm install hexo-cli -g```  //全局安装hexo手脚架

```hexo init <dir>```  //dir是你需要安装到的路径和项目名称比如我需要安装到D盘下的Project文件夹下,项目文件夹叫HexoBlog,则该命令应该是:

```hexo init d:/Project/HexoBlog```

```cd <dir>``` //跳转到刚才安装的目录,例如我们这里应该是:

```
d:
cd d:/Project/HexoBlog```
```npm install```  //我这里在安装手脚架时自动安装了所有node_modules,如果你不确定你是否完整安装了所有开发所需modules,可以手动再运行一次.

```hexo server ``` //运行一个简易的本地服务器预览生成的页面,默认的地址是http://localhost:4000/


到这里位置,Hexo已经安装好了

## 添加文章

在cmd中,指向当前目录的情况下,可以通过以下命令来新建帖子或页面:

```hexo new [layout] <title>```

比如以下命令会新建一篇标题为"HelloWorld"的帖子,因为默认的layout为post类型:

```hexo new HelloWorld```
