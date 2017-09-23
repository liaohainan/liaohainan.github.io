---
title: axios发送数据放在formdata里
date: 2017-1-18 21:14:55
tags: Vue
---

后端面向JAVA和PHP的时候接收ajax请求，他们比较喜欢要求把数据放在formdata里接收(.net表示放在哪里都可以)
而axios默认发送的是json数据，在request里，这时候需要转化一下

看过网上好多需要改Content-Type的，事实上并不需要


只需要在axios的config里处理下数据就好了


首先引入qs库，再加上transformRequest设置就ok了
```js

import axios from 'axios'
import qs from 'qs'

let config = {
    baseURL: '/api/',
    timeout: 10000,
    //`headers`选项是需要被发送的自定义请求头信息
    headers: {
        'X-Requested-With': 'XMLHttpRequest',
    },
    transformRequest: [function (data) {  
        //需要序列化数据，数据放到formdata 
        return qs.stringify(data)
    }]
}

```