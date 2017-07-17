---
title: Git出现 fatal:Pathspec 'xxx' is in submodule 'module/Cxxx' 异常 解决方案
date: 2017-01-02 22:12:25
tags: Other
---

百度过，谷歌过，没能解决，大概知道是子模块有自己的git仓库，所以不能添加到根目录里

## 这种方法好多人都说可以，但是我没成功
```shell
git rm -rf --cached CocktailMakerModule/
git add CocktailMakerModule/
```

## 还有一种
可以用小乌龟的添加子模块来解决，不过仓库还是区分了，并不是自己想要的结果，我想把子模块也放到主仓库里

## 来最直接有效的方法

删掉子模块内部的.git文件夹

然后把整个子模块文件夹都复制出来，然后在仓库里把子模块删掉

再把之前复制出去的文件夹整个粘贴回来

再执行git add就可以了
