---
title: ajax多次请求，只执行最后一次的方法
date: 2016-05-07 23:01:04
tags: JS
---



# ajax多次请求，只执行最后一次的方法

有时候点击按钮进行异步请求数据的时候可能网络差，用户会点击很多次，或者页面有很多相同的按钮，参数不同，但是调用的ajax相同，只想得到最后一次结果

我的思路是用闭包记录执行次数，并同时记录发起ajax的次数，等数据返回的时候比较两次次数的结果，渲染最后一次数据

多说无益，上代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="renderer" content="webkit">

    <script src="http://code.jquery.com/jquery-2.1.1.min.js"></script>
    <title>Title</title>
</head>
<body>
<button id="ajaxbtn">获取数据</button>
点击次数<span id="num"></span>
<div id="show">

</div>

<script>
    //定义点击次数和方法执行次数
    var a = 1;
    var flag = 1;
    $('#ajaxbtn').click(function () {
        a = a + 1
        $('#num').html(a)
        console.log(a);
        btnAjax('https://api.douban.com/v2/movie/in_theaters', cb);
    })
//封装ajax事件
    function btnAjax(url, cb) {

        $.ajax({
            type: 'get',
            url: url,
            dataType: 'jsonp',
            success: function (data) {
                var func = callbackFunc(data, cb);
                func()
            }
        })
    }
    //返回函数
    function cb(data) {
        console.log(a);
        console.log(data);
        var str = '';
        for (var i = 0; i < data.subjects.length; i++) {
            str += '<img src="' + data.subjects[i].images.small + '">';
        }
        $('#show').html(str)
    }
    //判断次数，获取返回函数
    function callbackFunc(data, cb) {
        flag++;
        if (a == flag) {
            return function () {
                cb(data);
            }
        } else {
            return function () {
            }
        }
    }
</script>
</body>
</html>
```