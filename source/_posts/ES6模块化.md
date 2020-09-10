---
title: ES6模块化
date: 2020-09-10 12:49:37
tags: 模块化
---


### 特点

1. 使用依赖**预声明**的方式导入模块
    - 依赖预声明
        优点：一开始就确定依赖关系
        缺点：某些时候效率较低
    - 依赖延迟声明：执行到require才加载模块
        优点：某些时候可以提高效率
        缺点：无法在一开始确定依赖关系（比较模糊）
    
2. 灵活的多种导入导出方式 
3. 规范的路径表示法：所有路径必须以./或../开头

### 模块的引入

**注意：这一部分非模块化标准**

目前，浏览器使用以下方式引入一个ES6模块文件

`<script src="入口文件" type="module"></script>`

### 分类

ES6中的模块导入导出分为两种：
1. 基本导入导出
2. 默认导入导出

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f154c79ccc6f4d16b0677218d514c8ed~tplv-k3u1fbpfcp-zoom-1.image)


### 基本导入导出

#### 基本导出
基本导出必须有多个，每个必须有名称

基本导出的语法如下：
`export 声明表达式`

```js
export var a = 1; // 导出a，值为1，类似于CommonJS中的exports.a = 1;
export function test() { // 导出test，值为一个函数

}
export class Person { // 导出Person，值为一个类

}
```
或
`export { 具名符号 }`
```js
export { age, sex } // 将age变量的名称作为导出的名称，age变量的值作为导出的值

// 等效于
export var age = 18;
export var sex = '女';
```


#### 基本导入

由于使用的是**依赖预加载**，因此，导入任何其他模块，导入代码必须放置到所有代码之前

`import {导入的符号列表} from '模块路径'`

注意以下细节：
- 导入时，可以通过关键字as对导入的符号进行重命名
    `import {name as name2} from './a.js'`
- 导入时使用的符号是常量，不可修改
- 可以使用 * 号导入所有的基本导出，形成一个对象
    `import * as obj from './a.js'`
- 只运行模块，不使用它内部的任何导出 
    `import './a.js'`
    

### 默认导入导出

#### 默认导出

`export default 默认导出的数据`

由于每个模块仅允许有一个默认导出，因此，每个模块不能出现多个默认导出语句

#### 默认导入

`import 接收变量名 from '模块路径'` 

如果希望同时导入某个模块的默认导出和基本导出，可以使用下面的语法：
`import data, { a , b } from './a.js'`

将默认导出放到data里，基本导出分别放到变量a和b中 