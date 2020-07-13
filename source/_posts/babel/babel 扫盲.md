---
title: babel 扫盲
tags:
  - babel
  - JavaScript
abbrlink: 580f12e2
date: 2020-03-23 16:21:41
---
## ☆ babel是什么
Babel 是一个 JS 编译器,主要用于将 ECMAScript 2015+ 版本的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。

#### Babel了什么:
- 语法转换
- 通过 Polyfill 方式在目标环境中添加缺失的特性(@babel/polyfill模块)
- 源码转换(codemods)
- 
## ☆ Babel 的使用和配置
>  Babel 虽然开箱即用，但是什么动作也不做，如果想要 Babel 做一些实际的工作，就需要为其添加「插件」(plugin)。

### Babel 的插件

#### 插件的分类:
1、语法插件
> 解析（parse） 特定类型的语法（不是转换），可以在 AST 转换时使用，以支持解析新语法

2、转换插件
> 转换插件会启用相应的语法插件(因此不需要同时指定这两种插件)，这点很容易理解，如果不启用相应的语法插件，意味着无法解析，连解析都不能解析，又何谈转换呢？

#### 插件的使用
项目目录下新建 .babelrc 文件，发布在 npm 上的插件，直接填写插件的名称，或者指定插件的相对/绝对路径


### 预设
> 理解为一堆plugin（插件）的集合
```
@babel/preset-env
@babel/preset-flow
@babel/preset-react
@babel/preset-typescript
```
#### @babel/preset-env
> 作用是对我们所使用的并且目标浏览器中缺失的功能进行「代码转换」和「加载 polyfill」，@babel/preset-env包含的插件将支持所有最新的JS特性(ES2015,ES2016等，不包含 stage 阶段)

```
//.babelrc
{
    "presets": ["@babel/preset-env"]
}

```
@babel/preset-env 会根据你配置的目标环境，生成插件列表来编译。基于浏览器或 Electron 的项目，官方推荐使用 .browserslistrc 文件来指定目标环境


#### Polyfill

==@babel/polyfill== 模块包括 ==core-js== 和一个自定义的 ==regenerator runtim==e 模块，可以模拟完整的 ES2015+ 环境

可以使用诸如 ==Promise== 和 ==WeakMap== 之类的新的内置组件、 Array.from 或 Object.assign 之类的静态方法、Array.prototype.includes 之类的实例方法以及生成器函数(前提是使用了 @babel/plugin-transform-regenerator 插件)。为了添加这些功能，polyfill 将添加到全局范围和类似 String 这样的内置原型中(会对全局环境造成污染，后面我们会介绍不污染全局环境的方法)。

> 补充说明 V7.4.0 版本开始，@babel/polyfill 已经被废弃(前端发展日新月异)，需单独安装 core-js 和 regenerator-runtime 模块。

1 安装 @babel/polyfill
```
npm install --save @babel/polyfill
// 不使用 --save-dev，因为这是一个需要在源码之前运行的垫片

// 需要在其它代码之前引入
entry:[
    require.resolve('./polyfills')
    path.resolve('./index')
]
``` 

2、按需加载polyfill, 仅仅把需要的 polyfill 包含进来。

我们未必需要完整的 @babel/polyfill（89k v7.7.0）,@babel/preset-env 提供了一个 useBuiltIns 参数，设置值为 usage 时,配置此参数的值为 usage ，必须要同时设置 corejs, 推荐使用core-js@3
```
npm install --save core-js@3
```
> [core-js (点击了解更多)](https://github.com/zloirock/core-js) : JavaScript 的模块化标准库，包含 Promise、Symbol、Iterator和许多其他的特性，它可以让你仅加载必需的功能

```
//.babelrc
const presets = [
    [
        "@babel/env",
        {
            "useBuiltIns": "usage",
            "corejs": 3
        }
    ]
]
```
> Babel 会检查所有代码，以便查找在目标环境中缺失的功能，然后仅仅把需要的 polyfill 包含进来。
## 参考文档
【1】[https://juejin.im/post/5ddff3abe51d4502d56bd143](https://juejin.im/post/5ddff3abe51d4502d56bd143)