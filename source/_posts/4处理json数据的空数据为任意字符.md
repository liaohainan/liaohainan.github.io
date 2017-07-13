---
title: 处理json数据的空数据为任意字符
date: 2016-05-06 22:06:42
tags: JS
---

# 处理json数据的空数据为任意字符

有时候从后台返回来的数据需要处理一下，根据实际开发需求，不能在页面上直接显示空字符，需要显示为“无内容”或者其他字段，而有些json数据结构比较深，就需要用到遍历和递归进行检查数据来达到这个目的，当然这个方法比较初级，会将所有的空字段都处理成你想要的结果，

 

如果遇到后台未返回相关字段的情况，就需要手动为返回的数据进行添加属性，这就另当别论了

```js
// 处理ajax里的空字段
function dataCheck(value, message) {
    /*
     * value需要处理的数据
     * message将空数据改成的数据
     * */
    var message = message || "无";
    for (var key in value) {
        if (!$.trim(value[key]) || $.trim(value[key]) == "") {
            value[key] = message;
        } else if (isArray(value[key])) {
            for (var i = 0; i < value[key].length; i++) {
                dataCheck(value[key][i]);
            }
        }
    }
    return value;//返回处理完的数据
}

```

