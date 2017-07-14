---
title: vue手机项目在手机端调试
date: 2017-2-15 21:36:22
tags: Vue
---

用vue-cli创建的项目在

npm run dev后会自动打开浏览器localhost:8080看到效果

有时候遇到手机上的项目，需要在手机上进行调试，这时候手机上就访问不到localhost了

这时候我们只需要更改一下
build/dev-server.js

将var uri = 'http://localhost:' + port
更改为var uri = '你本机的IP:' +port

![](/img/vue1.png)

**重启项目！！！**


这样浏览器自动打开的就是本机地址了，将这个地址发到手机上，在手机上（前提是手机和电脑处在同一网络中）就可以访问了


有时候项目打开的多了，会造成端口冲突，开可以在
config/index.js里将port随便改个其他的端口


如果是调试js可以使用腾讯开源的vconsole

https://github.com/WechatFE/vConsole

![](/img/log1.png)

或者是spy-debugger（还可以调试布局）

https://github.com/wuchangming/spy-debugger

![](/img/log2.jpg)

