---
title: vue根据后台返回01显示男女
date: 2016-12-13 21:14:18
tags: Vue
---




vue根据后台返回01显示男女

```js
//性别显示转换
//在计算属性里写好如下类似filter的代码
formatSex: function (sex) {
    return sex == 1 ? '男' : sex == 0 ? '女' : '未知';
},

```           