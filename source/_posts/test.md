---
title: 模块化-CommonJS
date: 2020-08-31 17:02:37
tags: 模块化
---

## 前期准备工作

安装nodejs

### 概述

在nodejs中，由于**有且仅有一个入口文件**（启动文件），即启动的时候只能启动一个js文件，而开发一个应用一个文件肯定不够，肯定会涉及到多个文件配合来完成整个应用的功能，这个时候就需要模块化。

把一个文件当成一个模块的话，多个文件就是多个模块，那么，各个模块之间如何配合、 引用就是模块化要做的事。

因此，nodejs对模块化的需求比对浏览器大的多。因为浏览器里面可以用多个js文件，而nodejs只能启动一个文件，而开发的时候肯定会涉及多个文件，因此，就必须要有模块化来管理这种依赖。比如说，下图：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a0161622b4954bba87ed05611924fcab~tplv-k3u1fbpfcp-zoom-1.image)

分析上图：

- 比如说，我们的应用里有5个文件

- 启动文件是index.js

- 还有一些其他功能的文件（a.js、b.js等）

- 这些文件各有各的功能，并且形成依赖关系，比如说：在启动入口文件里面要用到a.js和c.js，该如何用？a.js又要用到b.js和c.js，这又该如何用？

- 如何形成这种关系，就是模块化要形成规范的问题。

由于nodejs刚刚发布的时候，前端没有统一的、官方的模块化规范，因此，它选择使用社区的CommonJS作为模块化规范。

在学习CommonJS之前，首先认识两个重要的概念：**模块的导出**和**模块的导入**。

### 模块的导出

#### 什么是模块

> 模块就是一个js文件，它实现了一部分功能，并隐藏自己的内部实现，同时提供一些接口供其他模块使用。

举例说明上面一段话：

第一种情况:  util.js 是入口文件

    ```JS
    var count = 0;
    function getNumber(){
        count ++;
        return count;
    }
    console.log(getNumber());
    console.log(getNumber());
    console.log(getNumber());
    console.log(getNumber());
    ```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e4c620ea8037473ea566e4a63284ffd3~tplv-k3u1fbpfcp-zoom-1.image)


第二种情况  util.js 是入一个模块，被其他文件使用

1. 这个时候要考虑一个问题：这个模块里面有很多代码，有一些东西是希望别人使用的，有一些是不希望别人使用的；
2. 比方说：函数getNumber是希望别人使用的，是要暴露出去的；
3. 比方说：count，是要内部隐藏的，如果暴露出去会造成变量污染，也让整    个接口（接口：比如数据、对象等希望给外部使用的东西）变得复杂；
4. 所以，只需要把方法暴露出去，这个时候就需要模块化来解决这个问题：如何 隐藏自己 和 如何暴露给外部使用？

  ```js
  var count = 0; // 需要隐藏的内部实现

  // 要暴露给外部的接口（接口：比如数据、对象等希望给外部使用的东西）
  function getNumber(){
      count ++;
      return count;
  }
  console.log(getNumber());
  ```

#### 模块的两个核心要素：隐藏和暴露

- 隐藏的，是自己内部的实现
- 暴露的，是希望外部使用的接口

任何一个正常的模块化标准（比如c语言，csharp），都应该默认隐藏模块中的所有实现，而通过一些语法或api调用来暴露接口。

举例说明上面一段话：

    ```js
    var count = 0; // 需要隐藏的内部实现

    // 要暴露给外部的接口（接口：比如数据、对象等希望给外部使用的东西）
    function getNumber(){
        count ++;
        return count;
    }
    console.log(getNumber());
    ```

分析：
默认情况下，以上代码全部隐藏，其他模块无法使用该模块，该模块内部的变量、函数都不会造成全局的污染，都是内部的实现；

若要暴露给外部使用需要通过特殊的语法或一些api（api：比如调用某个函数，或者给某个对象赋值）

比如说，以上代码没有使用任何方式，所以，变量count和方法getNumber都是隐藏的。

再次用图来说明 模块化

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/db902903f1874be88d1c6f9a51180b85~tplv-k3u1fbpfcp-zoom-1.image)

如何理解上图：

比如说有一个模块 util.js，自己的内部实现需要隐藏起来，不希望外部使用；另外，希望外面使用的东西通过某种方式暴露出去，当然，可能会包含多个东西，比如说：暴露一个函数（sort）、 暴露一个对象（obj）、暴露一个字符串（name），暴露一个或多个都可以。

这就是模块化要解决的问题：如何隐藏？如何暴露？

***暴露接口的过程即模块的导出***

比如说，要把getNumber函数暴露出去，就可以说把它导出。

### 模块的导入

当需要使用一个模块时，使用的是该模块暴露的部分（导出部分），隐藏的部分是永远无法使用的。  

> 当通过某种语法或api去使用一个模块时，这个过程叫做模块的导入。

### CommonJs规范

CommonJs使用exports导出模块，require导入模块。

具体规范如下：

1. 如果一个JS文件中存在exports或require，该JS文件是一个模块
2. 模块内的所有代码均为隐藏代码，包括全局变量、全局函数，这些全局的内容均不应该对全局变量造成任何污染
3. 如果一个模块需要暴露一些api给外部使用，需要通过exports导出，exports是一个空的对象，你可以为该对象添加任何需要导出的内容

    ```js
    // exports = {}
    exports.getNumber = getNumber

    // 相当于
    /*
     exports: {
         getNumber: getNumber,
         abc: 123
     }
     */
    ```

4. 如果一个模块需要导入其他模块，通过require实现，require是一个函数，传入模块的路径即可返回该模块导出的整个内容

    ```js
    var util = require('./util.js')
    console.log(util)
    console.log(util.getNumber()) // 调用
    console.log(util.getNumber()) // 调用
    console.log(util.getNumber()) // 调用
    console.log(util.count) 
    ```

    ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0629dea7d01141db8e9669c442ac2a8d~tplv-k3u1fbpfcp-zoom-1.image)

### nodejs对CommonJs的实现

为了实现CommonJs规范，nodejs对模块做出了以下处理：

1. 为了保证高效的执行，仅加载必要的模块。nodejs只有加载到require函数时才会加载并执行模块

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cc152966e0b742ffa802bf8ed910f0d5~tplv-k3u1fbpfcp-zoom-1.image)

  分析：nodejs做了啥？
  首先，require函数读取到内部路径，然后运行该js，并且把里面导出的内容形成一个对象，作为requre函数的导出结果

  细节：nodejs中导入模块，使用相对路径，并且必须以 ./ 或 ../开头

2. 如何隐藏？

    为了隐藏模块中的代码，nodejs执行模块时，会将模块中的所有代码放置到一个函数中执行，以保证不污染全局变量。
    可以理解为匿名函数，其实不是匿名函数：

    ```js
    (function(){
        // 模块中的代码
    }())
    ```

3. 如何导出？
    为了保证顺利的导出模块内容，nodejs做了以下处理：
    1. 在模块开始执行之前，初始化一个值 module.exports = {}
    2. module.exports即模块的导出值
    3. 为了方便开发者便捷的导出，nodejs在初始化完module.exports后，又声明了一个变量exports = module.exports

    举个伪代码例子：

      ```js
      (function (module) {
          // 1.
          module.exports = {}

          // 2.
          var exports = module.exports;

          var count = 0;
          exports.getNumber = function() {
              count++;
              return count;
          }
          exports.abc = 123;

          // 3.
          return module.exports;
      }())
      ```

    模块导出的多种方式：
    - 第一种：给exports加属性导出(注意：不是直接赋值)
    - 第二种：直接给module.exports加属性导出

        ```js
        var count = 0;
        module.exports.getNumber = function() {
            count++;
            return count;
        }
        module.exports.abc = 123;
        ```

    - 第三种：直接给module.exports赋值

        ```js
        console.log(module.exports === exports) // true 都是空对象
        var count = 0;
        module.exports = {
            getNumber: function () {
                count++;
                return count;
            },
            abc: 123
        }
        console.log(module.exports === exports) // false
        // module.exports 有值
        // exports 还是空对象
        ```

        选择哪种导出方式看个人习惯，个人习惯第三种-直接给module.exports赋值。

4. 为了避免反复加载同一个模块，nodejs默认开启了模块缓存，如果加载的模块已经被加载过了，则会自动使用之前的导出结果。