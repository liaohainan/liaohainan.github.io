---
title: tab栏切换，内容为不断实时刷新数据的vue实现方法
date: 2017-02-07 20:11:34
tags: Vue
---

先说一下产品需求，就是有几个tab栏，每个tab栏对应的ajax请求不一样，内容区域一样，内容为实时刷新数据，每3s需要重新请求，返回的数据在内容区域展示，每点击一次tab栏需停止其他tab栏ajax请求，防止阻塞，首次加载页面的时候又不能5个ajax同时请求，只需要请求第一个就好

也没有必要建立5个区域，控制显示隐藏，浪费性能，业务代码就不贴了，把大概原理的代码贴上来

![](img/7.gif)


先是用jq实现了一版

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="renderer" content="webkit">
    <title>Title</title>
    <script src="http://code.jquery.com/jquery-2.1.1.min.js"></script>
   
</head>
<body>

<div>
    <ul>
        <li>click</li>
        <li>click</li>
        <li>click</li>
        <li>click</li>
        <li>click</li>
    </ul>
</div>

<script>
    var arr=[
        function(){console.log(0);},
        function(){console.log(1);},
        function(){console.log(2);},
        function(){console.log(3);},
        function(){console.log(4);}
    ];

    var seti=setInterval(arr[0],1000)
    $('li').click(function(){
        clearInterval(seti)
        seti=setInterval(arr[$(this).index()],1000)
    })

</script>

</body>
</html>
```



再看vue版

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="renderer" content="webkit">
    <title>Title</title>
   
    <script src="https://unpkg.com/vue@2.2.6/dist/vue.min.js"></script>
</head>
<body>
<div id="vm">
    <button @click="tab(0)">click0</button>
    <button @click="tab(1)">click1</button>
    <button @click="tab(2)">click2</button>
    <button @click="tab(3)">click3</button>
    <button @click="tab(4)">click4</button>
    <div>
        <p>{{show}}</p>
    </div>
</div>
<script>
    var vm1 = new Vue({
        el: '#vm',
        data: {
            arr:[
                function(){console.log(0);},
                function(){console.log(1);},
                function(){console.log(2);},
                function(){console.log(3);},
                function(){console.log(4);}
            ],
            time1:'',
            time2:'',
            show:'',
            num:0,

        },
        methods: {
            tab: function(index){
                //如果每个tab的方法不一样，提前用一个数组把方法保存起来
                clearInterval(this.time1)
                this.time1=setInterval(this.arr[index],1000)
                //以下是调用同一种方法的时候可以不需要设置数组
                this.num = 0
                clearInterval(this.time2)
                this.time2 = this.fun(index)
            },
            fun:function(index){
                var _this=this;
                return setInterval(function(){
                    //写个随机数显示在页面，具体业务需求应该是ajax请求
                    var random=String(parseInt(Math.random()*100000000000))
                    //字符一个一个显示在页面上
                    _this.show=index+random.substring(0,_this.num++);
                },300)

            }
        },
        mounted:function(){
            this.time1=setInterval(this.arr[0],1000)
        }
    });


</script>
</body>
</html>
```