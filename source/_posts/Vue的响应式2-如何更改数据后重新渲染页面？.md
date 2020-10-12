---
title: Vue的响应式2-如何更改数据后重新渲染页面？
date: 2020-10-12 19:07:05
tags: vue
---


### 不渲染页面的情况
#### 1. 数组-通过索引的方式更改数组
```html
<div id="app">
    {{ arr }}
</div>
```
```js
const vm = new Vue({
    el: '#app',
    data: {
        arr: ['a', 'b', 'c']
    }
})
```
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d4b6e1c6752043c2809cb520b4e5b769~tplv-k3u1fbpfcp-watermark.webp)

#### 2. 数组-更改长度
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5201c9a198ba42ab953b06b6d6ae0823~tplv-k3u1fbpfcp-watermark.webp)

#### 3. 对象-增属性
```html
<div id="app">
    {{ mrKen.wife }}
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
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/60ee8958f199449d9bb546b471eaee43~tplv-k3u1fbpfcp-watermark.webp)


#### 4. 对象-删属性
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9aa5bc03c8f74545848c51b51f421e30~tplv-k3u1fbpfcp-watermark.webp)

### 渲染页面的情况
#### 1. 数组-变异方法
push、pop、shift、unshift、splice、sort、reverse

#### 2. vm.$set/Vue.set(改哪个、要改的值、改成什么)

```js
// 把arr = ['a', 'b', 'c']的第三项改成'd'
vm.$set(vm.arr, 2, 'd');

// 把mrKen = {name: '垦哥',age: 30}的'age'属性值改成18
vm.$set(vm.mrKen, 'age', 18);
```

#### 3. vm.$delete/Vue.delete(删哪个、要删的值)
```js
// 删除arr = ['a', 'b', 'c']的第三项
vm.$set(vm.arr, 2);

// 删除mrKen = {name: '垦哥',age: 30}的'age'属性
vm.$set(vm.mrKen, 'age');
```