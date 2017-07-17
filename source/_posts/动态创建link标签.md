---
title: 动态创建link标签
date: 2016-09-02 22:24:14
tags: HTML
---

虽然现在流行响应式，自适应等等，但是仍旧有些系统存在换肤啦，切换样式啦等需求，这样可以通过动态创建link标签来实现这样的效果
废话不多说，上代码


```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="renderer" content="webkit">
    <script src="../../base/jquery-1.12.2.js"></script>
    <title>Title</title>
</head>
<body>
<button id="pc">pc</button>
<button id="mob">mob</button>
<button id="remove">remove</button>

<script>

    $('#pc').click(function () {
        link.creat('ccss', 'pc.css')
    })
    $('#mob').click(function () {
        link.creat('ccss', 'mob.css')
    })
    $('#remove').click(function () {
        link.remove('#ccss')
    })


    var link = {
        remove: function (id) {
            $('link').remove(id);
        },
        //单例模式写法
        creat: (function () {
            var linkTag;
            return function(id,url){
                if (document.getElementById(id)) {
                    linkTag.attr({'id':id,'href':url})
                }else{
                    linkTag = $('<link>').attr({
                        'id': id,
                        'href': url,
                        'rel': 'stylesheet',
                        'type': 'text/css',
                        'media': 'all'
                    });
                    $($('head')[0]).append(linkTag);
                }
                return linkTag;
            }
        })()
    }


</script>
</body>
</html>
```