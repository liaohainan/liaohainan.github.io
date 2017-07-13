---
title: hexo使用方法
date: 2016-05-02 10:14:50
tags: Other
---
# 正式安装Hexo

Node和Git都安装好后,首先创建一个文件夹,如blog,用户存放hexo的配置文件,然后进入blog里安装Hexo。

执行如下命令安装Hexo：
```shell
sudo npm install -g hexo
```
初始化然后，执行init命令初始化hexo,命令：
```shell
hexo init
```
好啦，至此，全部安装工作已经完成！blog就是你的博客根目录，所有的操作都在里面进行。

生成静态页面
```shell
hexo generate（hexo g也可以）
```
本地启动

启动本地服务，进行文章预览调试，命令：
```shell
hexo server
```
浏览器输入http://localhost:4000

我不知道你们能不能，反正我不能，因为我还有环境没配置好

# 配置Github

建立Repository

建立与你用户名对应的仓库，仓库名必须为【your_user_name.github.io】，固定写法

然后建立关联，我的blog在本地/Users/leopard/blog，blog是我之前建的东西也全在这里面，有：
```
    _config.yml    node_modules    public      source

    db.json        package.json    scaffolds  themes
```
现在我们需要_config.yml文件，来建立关联，命令：
```
vim _config.yml
```
翻到最下面，改成我这样子的
```shell
deploy:

     type: git

     repo: https://github.com/leopardpan/leopardpan.github.io.git

     branch: master
```
然后执行命令：
```shell
npm install hexo-deployer-git --save
```
网上会有很多说法，有的type是github, 还有repository最后面的后缀也不一样，是github.com.git，我也踩了很多坑，我现在的版本是hexo: 3.1.1，执行命令hexo -vsersion就出来了,貌似3.0后全部改成我上面这种格式了。

忘了说了，我没用SSH Keys如果你用了SSH Keys的话直接在github里复制SSH的就行了，总共就两种协议，相信你懂的。

然后，执行配置命令：
```shell
hexo deploy
```
然后再浏览器中输入http://leopardpan.github.io/就行了，我的github的账户叫leopardpan,把这个改成你github的账户名就行了

# 部署步骤

每次部署的步骤，可按以下三步来进行。
```shell
    hexo clean

    hexo generate

    hexo deploy
```

# 一些常用命令：
```shell
hexo new "postName" #新建文章

hexo new page "pageName" #新建页面

hexo generate #生成静态页面至public目录

hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）

hexo deploy #将.deploy目录部署到GitHub

hexo help # 查看帮助

hexo version #查看Hexo的版本
```
链接：http://www.jianshu.com/p/465830080ea9
來源：简书

主题配置方法

https://github.com/iissnan/hexo-theme-next
