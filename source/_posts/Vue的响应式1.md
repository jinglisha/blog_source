---
title: Vue的响应式1
date: 2020-10-07 15:16:29
tags: vue
---


### 什么是vue的响应式？
> 当数据变化，页面重新渲染。

### 数据如何变化 ？如何更改vue中的数据？
> 创建vue对象实例的时候，vue会将data中的成员代理给vue实例，所以，可以直接用vm.xxx 的形式拿数据。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba298ac577a547628f2f94dbe91d3b03~tplv-k3u1fbpfcp-zoom-1.image)

1. 为什么把data作为vue实例的属性，而不是只有一个data属性，包含data里的所有数据？
    实现响应式，首先得知道数据变化了；如何知道数据变化了，得监控对象；
    直接监控第二层级比第一层级好监控。
    
    通过 Object.defineProperty 进行数据劫持 (vue2.0)
    
### $xxx和_xxx是什么？
> 都是vue内部自带的属性,主要防止命名冲突。

* $ 是暴露给外部使用的；
* _ 是vue内部使用的；

### 哪些数据更改后不能重新渲染页面？

- 一开始未经声明的变量;举例如下：height属性一开始未经声明，虽然后来添加了该属性，但页面没有重新渲染。

```html
<div id="app">
   {{ mrKen.height }}
</div>
```
```js
const vm = new Vue({
        el: '#app',
        data: {
            msg: '垦哥的基本信息',
            mrKen: {
                name: '垦哥',
                age: 30              
            }
        }
    })
```
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f4998f77bdbd4214b3973b04bf5d8081~tplv-k3u1fbpfcp-zoom-1.image)
    
 - 声明过，但未经使用
 
    未在dom区域使用过，更改该属性的值也不渲染页面 
    
    
### 更改数据后，页面会立刻重新进行渲染吗？
> 不会，因为渲染页面的操作是异步执行的。(先执行同步队列里的任务、再执行异步队列里的微任务、最后是宏任务。) 如果渲染页面是同步任务会卡死 

### $el 获取监控的dom

### vm.$nextTick() 拿到渲染页面之后的数据
如何判断页面渲染的文本与更改的数据是否一致？
> $nextTick() 微任务 当vue更新dom的操作完毕之后，会执行该函数里的方法。

```js
vm.$nextTick(() => {
    console.log(vm.msg, vm.$el.innerText);
})
``` 
当dom重新渲染完毕之后，会立即执行vm.$nextTick()；我们就能拿到最新的dom的值。

同Vue.nextTick()，都可作为Promise去使用。
```js
vm.$nextTick().then(() => {
    console.log(vm.msg, vm.$el.innerText);
    // 垦哥的基本信息 垦哥的基本信息
})

Vue.nextTick().then(() => {
    console.log(vm.msg, vm.$el.innerText);
    // 垦哥的基本信息 垦哥的基本信息
})
```
vm.$nextTick()和Vue.nextTick()的区别：
前者的this是vue实例；后者的this是window

这里不能用箭头函数，因为箭头函数的this是上一级有this的父元素或者祖先元素。

```js
vm.$nextTick(function(){
    console.log(this);
})

Vue.nextTick(function(){
    console.log(this);
})
```
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a83b3534b27d44a4a6b6b4123f36867b~tplv-k3u1fbpfcp-zoom-1.image)

使用vm.$nextTick()居多。方便使用this。

### nextTick()内部如何处理？

```js
if(typeof Promise !== 'undefined'){
         
} else if(typeof MutationObserver !== 'undefined'){

} else if(typeof setImmediate !== 'undefined'){

} else {
    // setTimeout()
}
```

1. 先执行微任务
    - Promise.resolve().then()
    - MutationObserver
    它的功能是突变观察，它会在vue源码的内部创建一个假的节点，通过改动假节点，触发函数
2. 再执行宏任务
    - setImmediate IE下,比setTimeout先执行
    - setTimeout

### vue机制上的小问题：vue总是在等主线程上的同步任务执行完毕，才去渲染页面；如果主线程上的任务卡死，页面就一直不渲染。vue不将主线程让出来。








