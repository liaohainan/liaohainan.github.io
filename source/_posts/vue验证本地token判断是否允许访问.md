---
title: vue验证本地token判断是否允许访问
date: 2017-1-19 22:16:22
tags: Vue
---

Vue官网给的例子是把token存到vuex里，但是我们知道vuex在用户刷新浏览器的时候数据就丢失了，造成拿不到token就又跳到登录页了，这显然是不可以的，于是我们只能换个地方那就是sessionStorage

看代码

```js

router.beforeEach((to, from, next) => {
  NProgress.start();
  if (to.path == '/login') {
    sessionStorage.removeItem('token');
  }
  let user = JSON.parse(sessionStorage.getItem('token'));
  if (!user && to.path != '/login') {
    next({ path: '/login' })
  } else {
    next()
  }
})
```

但是又遇到另外一个问题，虽然后台给token加了时间，比如半小时就失效，但是用户一直没有关闭，sessionStorage就一直没有消失，用户还是可以跳转到他想去的页面

所以我们需要在每个ajax的接口里都要验证token的有效性（后端验证)，根据后端返回的状态码进行判断

```js

if (res.returnCode === '401') {
    //没有权限，跳转到登录页
    this.$router.push({ path: 'login' })
    
}

```

现在普通浏览器都没有问题，但是在中国有这样神奇的浏览器，360双核浏览器，有时候莫名其妙的在IE和Chrome之间切换，sessionStorage在两个内核里不共享，这个时候可以考虑用cookie

不过只需要在index.html头部加一句
```html
<meta name=renderer content=webkit>
```

就可以了，强制使用webkit内核

