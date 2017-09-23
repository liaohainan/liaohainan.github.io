---
title: vue动态路由页面刷新问题
date: 2017-09-23 16:54:18
tags: Vue
---


有的时候使用动态路由，因为使用的是同一个组件，而routes没有变化，导致页面没有更新
今天在群里看到一个有趣的解决思路，特发上来

首先在app.vue的router-view上加一个key，然后计算属性里判断fullPath
这样就解决了

```html
<template>
    <div class="app">
        <router-view :key="key"></router-view>
    </div>
</template>
```

```js
computed:{
    key(){
        return this.$router.fullPath !== undefined
            ? this.$route.fullPath + new Date()
            : this.$route +new Date()
    }
}

```