---
title: vue上拉加载更多的一些性能优化
date: 2017-07-13 20:13:12
tags: Vue
---

vue上拉加载更多或者是下拉刷新的一些性能优化


正常的加载更多写法可能是这样的
```js
this.list.concat(res.data.list)
```

下拉刷新
```js
this.list = res.data.list
```
如果遇到复杂的情况我们只想拿到后台返回数据数组中的某一个值那只好用循环了

或许你能想到的方法是这样的
加载更多
```js
//这种写法如果层级比较高的话有可能出现数据更新视图不更新的情况
res.data.list.forEach(function (e, i) {
    this.list[this.list.length + i].name = e.name
})
//比较好的写法是用push
res.data.list.forEach(function (e, i) {
    this.list.push({name: e.name})
})
```
<!-- more -->
push虽然能保证视图更新了，但是每次push之后，理论上vue检测到变化内部set方法执行，然后进行视图更新，这样每次push就会调用一次视图更新

```js
//那么就有了如下的写法,只调用一次set
let arr =[]
res.data.list.forEach(function (e, i) {
    this.list.push({name: e.name})
})
this.list.concat(arr)
```

同理下拉刷新的时候也新建一个空的arr，然后赋值给list

如果是微博这种列表数据量非常大的情况，还要考虑视图内只显示几十条数据，在push的同时把划上去看不到数据列临时存储起来