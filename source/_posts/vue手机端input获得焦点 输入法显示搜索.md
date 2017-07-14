---
title: vue手机端input获得焦点 输入法显示搜索
date: 2017-06-17 20:14:44
tags: Vue
---
类似这种点击搜索框，然后输入法的回车按键自动变为搜索两个字
![](img/sousuo1.jpg)
我们这样写就实现了
```html

<template>
    <div id="shoplist">
        <form @submit.prevent="search">
            <input type="search" v-model="inputdata">
            <button type="submit" >search </button>
        </form>
    </div>
</template>
<script>
export default {
  name: 'uuu',
  data () {
    return {
      inputdata: ''
    }
  },
  methods: {
    search () {
      alert(this.inputdata)
    }
  }
}
</script>
<style>
    
</style>


```