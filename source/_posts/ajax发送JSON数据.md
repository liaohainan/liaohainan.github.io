---
title: ajax发送JSON数据
date: 2016-06-02 22:34:12
tags: JS
---


有时候发送数据量比较大，后端会要求以对象的方法发送，也就是直接发送一个json过去

这时候我们需要更改下contentType
data也需要处理一下

```js
$.ajax({
    type: 'post',
    url: 'http://127.0.0.1:4000/getdel',
    dataType: 'json',
    contentType: 'application/json; charset=utf-8',
    data: JSON.stringify({a:1,b:2}),
    success: function (data) {
        console.log(data);
    },
    error: function () {
        console.log('err');
    }
})

```