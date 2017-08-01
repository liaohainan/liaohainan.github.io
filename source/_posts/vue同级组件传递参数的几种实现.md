---
title: vue同级组件传递参数的几种实现
date: 2017-07-20 22:17:22
tags: Vue
---




## vuex

通过vuex在mutations里提交，另一个组件this.$store.state.xxx就可以获取了

## $emit

通过$emit触发父组件的事件，更改父组件的值，然后再通过props传递到另一个子组件
```js
this.$emit('event', data)     

```    

## ref

在另一子组件上写一个ref，然后就可以在其他子组件里通过this.$refs.method来调用组件内事件，来改变当前组件内的值

## event bus

创建另一个vue实例，来触发其他组件内的事件

或者使用现有的插件vue-bus 