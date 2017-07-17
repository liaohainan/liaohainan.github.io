---
title: cooking构建工具报错MSBUILD error MSB4132解决办法
date: 2017-04-6 23:12:16
tags: Vue
---


# cooking构建工具报错MSBUILD error MSB4132解决办法

最近学习cooking构建工具构建多页面应用的时候，在自己的笔记本上运行的好好的，项目在公司电脑上clone下来的时候，发现构建报错，逐条查错，试了好多方法也不行

最后在github上找到了答案，只是之前一直没找到中文答案，搜了好久，英文差真是不行啊

报错信息为：

error MSB4132: The tools version "2.0" is unrecognized. Available tools versions are "4.0"

 

可能在这个错误之前会显示其他错误，都是webpack的疑似错误，并没有什么卵用，也有的认为是sass的问题，其实也不是

 

终极解决方法就是

用管理员打开命令提示行工具：

```shell
npm install --global --production windows-build-tools
```
 

安装完之后再执行命令
```shell
npm config set msvs_version 2015 --global
```
 

关掉命令行工具，然后再npm run dev就成功了

 

 

gihub原文https://github.com/chjj/pty.js/issues/60