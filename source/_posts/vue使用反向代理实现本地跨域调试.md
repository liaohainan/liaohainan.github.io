---
title: vue使用反向代理实现本地跨域调试
date: 2017-01-16 23:11:11
tags: Vue
---


# vue使用反向代理实现本地跨域调试

和后端联调时总是会面对恼人的跨域问题，

之前都是改host，或者用代理软件等方法调试接口

现在使用vue-cli生成的开发环境，内部已经加入了http-proxy-middleware，直接在config/index.js中，把proxytable里面配置一下就可以本地调试后端的接口了

假如我们的接口地址为

http://10.153.29.17:8916/api/getData

那么我们的proxytable这里就可以配置为
```js
proxyTable: {
      /**
         * 其中 '/api' 为匹配项，target 为被请求的地址
         * 因为在 ajax 的 url 中加了前缀 '/api'，而原本的接口是没有这个前缀的
         * 所以需要通过 pathRewrite 来重写地址，将前缀 '/api' 转为 '/'
         * 如果本身的接口地址就有 '/api' 这种通用前缀，就可以把 pathRewrite 删掉
         */
      '/api': {
        target: 'http://10.153.29.17:8916',
        changeOrigin: true,
        pathRewrite: {
          '^/api': '/api'
        }
      }
    },


```

然后我们的ajax为

```js
axios.post('/api/getData',{
  firstName:'Fred',
  lastName:'Flintstone'
})
.then(function(res){
  console.log(res);
})
.catch(function(err){
  console.log(err);
});

```

虽然在浏览器控制台里看到的访问地址还是localhost，但是控制台已经不会报跨域的错误了