---
title: vue使用webpack打包cdn资源优化
date: 2017-06-20 20:19:17
tags: Vue
---

项目做的越来越大，引入的外部资源也会越来越多，vue-cli的配置是将所有js打包在一起，但是这样会造成js体积特别大

有时候像echarts，moment这种有cdn的资源可以考虑在webpack里配置一下，直接引入cdn资源或者下载到本地，使用静态资源，而不是都打包到一个js里


修改build/webpack.base.conf.js
添加externals项，与resolve同级


```js
resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
        //路径别名
        'vue$': 'vue/dist/vue.esm.js',
        '@': resolve('src'),
    }
},
externals: {
    // 指定别名
    "moment": 'moment'
},

```

然后在index.html里添加cdn
```html

<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.18.1/moment.min.js"></script> 
```

页面里只需要正常的引用即可

```js
import moment from 'moment'
```
