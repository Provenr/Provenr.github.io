---
title: babel 扫盲
tags:
  - babel
  - JavaScript
abbrlink: 580f12e2
date: 2020-03-23 16:21:41
---
## 前言
本篇文章的目的:
1. 了解Babel 是什么
2. babel使用和配置
3. @babel/runtime，@babel/polyfill，@babel/plugin-transform-runtime 之间的关系
4. 了解插件和预设

## ☆ babel是什么
> Babel 是一个 JS 编译器（ES6 to ES5单纯的语法转换工具），为了解决了在老版本浏览器使用新语法的问题
### 1、babel的核心库 —— @babel/core
Babel 的核心功能包含在 @babel/core 模块中。想要在项目中使用 babel 进行编译，必须安装 @babel/core，否则会报错。

### 2、Babel做了什么:
Babel = syntax + polyfill
- syntax: ==语法转译==  （箭头函数）
- polyfill(@babel/polyfill模块): ==在目标环境中添加缺失的API== （Set、Map)

## ☆ Babel 的使用和配置
>  Babel 虽然开箱即用，但是什么动作也不做，如果想要 Babel 做一些实际的工作，就需要为其添加「插件」(plugin)。babel支持多种格式的配置文件


#### （一）项目根目录下新建 [.babelrc 文件](https://www.babeljs.cn/docs/config-files#file-relative-configuration)
```
//.babelrc
{
    "presets": [
        "@babel/preset-env", 
        "@babel/preset-react"
    ],
    "plugins": [
        "@babel/plugin-proposal-class-properties",
        "@babel/plugin-syntax-dynamic-import"
    ]
}
```
#### （二）babel.config.js
> 希望以编程的方式创建配置文件 或者 希望编译 node_modules 目录下的模块，使用[babel.config.js](https://www.babeljs.cn/docs/config-files#project-wide-configuration)。
```
// babel.config.js
module.exports = function(api) {
  api.cache(true);
  const presets = [...];
  const plugins = [...];
  return {
      presets,
      plugins
  };
}
```
更多配置参考[babelrc 文档](https://www.babeljs.cn/docs/config-files#file-relative-configuration)

#### （三）package.json
```
{
    "name": "my-package",
    // .babelrc 中的配置信息
    "babel": {
        "presets": [],
        "plugins": []
    }
}

```
#### （四）.babelrc.js
> 与 .babelrc 配置相同，但是可以使用JS编写。

```
//可以在其中调用 Node.js 的API
const presets = [];
const plugins = [];

module.exports = { presets, plugins };

```


## ☆ Babel 的插件 和 预设（presets）
### 1、Babel 的插件

#### 插件的使用
> 插件的排列顺序很重要！！！

- 插件在 Presets 前运行。
- 插件顺序从前往后排列，先执行 @babel/plugin-proposal-class-properties，后执行 @babel/plugin-syntax-dynamic-import
- preset 的执行顺序是颠倒的(从后往前)，先执行 @babel/preset-react， 后执行 @babel/preset-env。


#### 插件的参数
插件和 preset 都可以接受参数，参数由插件名和参数对象组成一个数组。preset 设置参数也是这种格式。

```
{
  "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        "corejs": 3
      }
    ]
  ]
}

```

#### 插件的短名称
如果插件名称为 @babel/plugin-XXX，可以使用短名称@babel/XXX :
```
{
    "plugins":
        [
            "@babel/transform-arrow-functions" //同"@babel/plugin-transform-arrow-functions"
        ]
}
```
如果插件名称为 babel-plugin-XXX，可以使用短名称 XXX:
```
{
  "plugins": [
    "newPlugin", //同 "babel-plugin-newPlugin"  
    "@scp/myPlugin" //同"@scp/babel-plugin-myPlugin"
  ]
}

```

### 2、预设
> 理解为一堆plugin（插件）的集合
```
// 常用预设
@babel/preset-env
@babel/preset-flow
@babel/preset-react
@babel/preset-typescript
```
#### @babel/preset-env
> 对我们所使用的并且目标浏览器中缺失的功能进行「代码转换」和「加载 polyfill」，@babel/preset-env包含的插件将支持所有最新的JS特性(ES2015,ES2016等，不包含 stage 阶段)

```
//.babelrc
{
    "presets": ["@babel/preset-env"]
}

```
@babel/preset-env 会根据你配置的目标环境，生成插件列表来编译。基于浏览器或 Electron 的项目，官方推荐使用 .browserslistrc 文件来指定目标环境

## ☆ Polyfill、@babel/plugin-transform-runtime、@babel/runtim 关系
#### Polyfill
> Polyfill = core-js  +  @babel/plugin-transform-regenerator

==@babel/polyfill== 模块包括 ==core-js== 和一个自定义的 ==regenerator runtim==e 模块，可以模拟完整的 ES2015+ 环境

1、安装 @babel/polyfill
```
npm install --save @babel/polyfill
// 不使用 --save-dev，因为这是一个需要在源码之前运行的垫片

// 需要在其它代码之前引入
app.js
import "@babel/polyfill";

``` 
> #### 补充说明 (2020/01/07)：V7.4.0 版本开始，@babel/polyfill 已经被废弃(前端发展日新月异)，需单独安装 core-js 和 regenerator-runtime 模块。因为@babel/polyfill只包含core-js 2.0的版本，但是core-js@2.0的版本已经在一年半之前冻结，所有的新特性只会添加到3.0的分支中

```
@babel/polyfill  替代方式

npm install --save core-js@3

// 单独引入
import "core-js/stable";
import "regenerator-runtime/runtime"; // 需要使用 generator 函数
```

2、按需加载polyfill, 仅仅把需要的 polyfill 包含进来。

我们未必需要完整的 @babel/polyfill（89k v7.7.0）,@babel/preset-env 提供了一个 useBuiltIns 参数，设置值为 usage 时,配置此参数的值为 usage ，必须要同时设置 corejs, 推荐使用core-js@3

> [core-js (点击了解更多)](https://github.com/zloirock/core-js) : JavaScript 的模块化标准库，包含 Promise、Symbol、Iterator和许多其他的特性，它可以让你仅加载必需的功能

配合 @babel/preset-env  一起使用：
```
//.babelrc
const presets = [
    [
        "@babel/env",
        {
            "useBuiltIns": "usage",
            "corejs": 3,
            targets: {
                browsers: ["last 2 versions", "not ie <= 10"]
            }
        }
    ]
]
```
推荐 【按需引入】 或者 【使用 useBuiltIns】
- useBuiltIns 的参数为 'usage', 仅为每个文件添加其使用的polyfill
> 入口处不需要手动导入 corejs | regenerator-runtime
- useBuiltIns 的参数为 'entry'，替换（或打散）为适配目标环境的各个引用
> 入口处需要手动导入 corejs | regenerator-runtime，
- useBuiltIns 未设置 或者 值为false，在入口文件引入的 polyfill 不做任何处理

#### @babel/plugin-transform-runtime
> polyfill 的另一种方式。

引入了 corejs、regenerator-runtime 这样的 polyfill 会有一定的副作用，比如：

- 引入了新的全局对象：比如Promise、WeakMap等。
- 修改现有的全局对象：比如修改了Array、String的原型链等。


在应用开发中, 直接引入了 corejs、regenerator-runtime 这样的 polyfill 是可以的。 但如果在库、工具的开发中推荐使用 @babel/transform-runtime 它将开发者依赖的全局内置对象等，抽取成单独的模块，并通过模块导入的方式引入，避免了对全局作用域的修改（污染）。



另外，==@babel/plugin-transform-runtime== 需要和 ==@babel/runtime== 配合使用。
```
npm install --save-dev @babel/plugin-transform-runtime // 开发时使用
npm install @babel/runtime-corejs3
npm install --save @babel/runtime // 生产依赖
```

```
//.babelrc
{
    "plugins": [
        [
            "@babel/plugin-transform-runtime",
            {
                corejs: 3,
            }
        ]
    ]
}
```
##### -对ECMAScript提案的API进行模拟
```
//.babelrc
{
    "plugins": [
        [
            "@babel/plugin-transform-runtime",
            {
               corejs: { version: 3, proposals: true },
            }
        ]
    ]
}

```

## 补充知识
monorepos 包的拆分

core-js@2一个最常见的问题就是包的体积太大(~2M)，并且有很多重复的文件被引用。基于这个原因，core-js@3对包进行拆分，三个核心的包分别是
- [core-js](https://www.npmjs.com/package/core-js)：定义全局的polyfill（~500k, 40k minified and gzipped）
- [core-js-pure](https://www.npmjs.com/package/core-js-pure)：提供不污染全局环境的polyfill，等价于core-js@2/library（~440k）
- [core]-js-compat](https://github.com/zloirock/core-js/tree/master/packages/core-js-compat)：包含了core-js模块和API必要的数据，通过browserslist来生成所需要的core-js模块的列表

## 参考文档
【1】[https://juejin.im/post/5ddff3abe51d4502d56bd143](https://juejin.im/post/5ddff3abe51d4502d56bd143)

【2】[致我们学前端的小时光—corejs与env、runtime的不解之缘](https://juejin.im/post/5ce693b45188252db303ff23#heading-10)