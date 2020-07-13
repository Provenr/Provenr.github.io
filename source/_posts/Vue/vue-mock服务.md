---
title: vue mock服务
date: 2019-06-16 19:22:45
tags: [vue, mock]
---
使用mockjs的有两种方式


### 1. 在项目入口文件中直接引入配置好的mock文件
1. 需要在项目内引入mock并创建相关配置文件
2. 直接拦截请求，NetWork看不到请求
3. 调用真实接口时，需要删除相关配置文件。

第一步：安装mockjs

```
# 安装
npm install mockjs -d
```

第二步：在项目src 目录下新建如下图mock文件夹
```
├── src
│   ├── mock
│   	├── userInfo.js          // 用户的mock数据配置
│   	├── index.js            // 模块整合导出Mock

```
1) index.js
```
import Mock from 'mockjs'
import userInfoAPI from './userInfo'

Mock.setup({
  timeout: '200-600'
})

//  get 方法 可能传参数 拼接到链接后面，采用正则匹配去拦截请求
Mock.mock(/phpApi\/user\/userinfo(|\?\S*)$/, 'get', userInfoAPI.getUserInfo)

export default Mock
```
2）userInfo.js
 详细使用方法参考上面[mock 官网](http://mockjs.com/)

```
import Mock from 'mockjs' // 获取random对象，随机生成各种数据，具体请翻阅文档
const Random = Mock.Random
const list = [] // 用于存放数据的数组
const code = 200 状态码
for (let i = 0; i < 10; i++) {
  let item = Mock.mock(
    {
      id: '@id',
      title: Random.csentence(10, 25), // 随机生成长度为10-25的标题
      img: "@image('200x200','red','#fff','img')",
      description: '@paragraph',
      icon: Random.dataImage('250x250', '文章icon'), // 随机生成大小为250x250的图片链接
      date: Random.date() + ' ' + Random.time() // 随机生成年月日 + 时间
    }
  )
  list.push(item)
}

const userinfo = Mock.mock({
  id: '@id',
  email: '@email',
  ip: '@ip',
  author: Random.cname(), // 随机生成名字
  avtor: "@image('200x200','red','#fff','img')"
})
// 返回状态码和用户数据
export default {
 getUserInfo: () => {
    return {
      code,
      list: userinfo // list： axios 接口请求拦截这么配置，可以根据自己情况配置
    }
  }
}
```

第三步：项目主入口main.js引入mock
```
import './mock'
```

第四步：axios 封装 [axios 官网](https://www.kancloud.cn/yunye/axios/234845)

request.js
```
import axios from 'axios'
import qs from 'qs'
import { Toast } from 'vant' //请求前loading
import store from '@/store'
import router from '@/router'
// 创建 axios 接口
const service = axios.create({
  baseURL: process.env.SERVICE_PHP_URL,
  timeout: 10000,
  responseType: 'json',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
})

// request拦截器
service.interceptors.request.use(config => {
  Toast.loading({
    loadingType: 'spinner',
    mask: false,
    duration: 0,
    message: '加载中...'
  })
  if (config.method !== 'get') {
    if (router.history.current.path !== '/login') {
      config.params = { 'token': store.getters.token || '' }
    }
  }
  config.data = qs.stringify(config.data)
  return config
}, error => {
  Promise.reject(error)
})

// respone拦截器
service.interceptors.response.use(response => {
  let res = response.data
  Toast.clear()
  if (res.code === 200) {
    return res.list
  } else {
    Toast(res.msg)
    // 判断登录过期 重定向登录页面
    if (res.code === 209) {
      // store.dispatch('LogOut').then(() => {
      //   this.router.replace({ path: '/login' })
      // })
    }
  }
  return Promise.reject(res.msg)
}, error => {
  Toast.clear()
  console.log(error)
  return Promise.reject(error)
})
export function get (url, params = {}, type = true) {
  return new Promise((resolve, reject) => {
    if (type) {
      phpService.get(url, {
        params: params
      }).then(response => {
        resolve(response)
      })
        .catch(err => {
          reject(err)
        })
    } else {
      javaService.get(url, {
        params: params
      }).then(response => {
        resolve(response)
      })
        .catch(err => {
          reject(err)
        })
    }
  })
}

```
在src 目录下新建api文件夹对请求接口统一管理

目录结构如下

```
├── src
│   ├── api
│   	├── user.js     // 用户的
```
```
import { post, get } from '@/utils/request'


// 用户信息
export function getUserInfo () {
  return get('/user/userinfo')
}
```

第五步：组件中使用
```
import { getUserInfo } from '@/api/user'

  getUserInfo().then(function (res) {
      // that.userinfo = res
      console.log(res)
    })

```

getUserInfo()请求接口为/user/userinfo，

axios 请求封装时设置
```
baseURL: process.env.SERVICE_PHP_URL,

//process.env.SERVICE_PHP_URL 在config配置文件中为 ‘phpApi’
```
所以mock 拦截请求的正则匹配

```
Mock.mock(/phpApi\/user\/userinfo(|\?\S*)$/, 'get', userInfoAPI.getUserInfo)
```
最终直接返回mock造的假数据。
![image](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/6BFCC147824D48D39D05749DE1ECD6F3/1987)

### 2. 配合express启一个服务
1. 模拟了真实的接口，
2. netWork可以看到请求
3. 不用在项目中做任何配置（正常请求后台接口）


第一步：新建service文件夹
```
npm init 初始化

填写package.json 文件

```

安装mockjs

```
# 安装
npm install mockjs -d

# 安装express
npm install express -d
```

第二步：在项目src 目录下新建如下图mock文件夹
```
│ ├── mock
│  	├── userInfo.js          // 用户的mock数据配置
│  	├── index.js
```

index.js 文件

```
// 引入express
const express = require('express')
// 引入mock
const Mock = require('mockjs')
// 实例化express
const app = express()

// post请求体相关
// 作用是对post请求的请求体进行解析
let bodyParser = require('body-parser')
// mock数据API
let articleAPI = require('./article')
app.use(bodyParser.json())

// 允许跨域
app.all('*', function (req, res, next) {
  res.header('Access-Control-Allow-Origin', '*')
  res.header('Access-Control-Allow-Methods', 'PUT, GET, POST, DELETE, OPTIONS')
  // 此处根据前端请求携带的请求头进行配置
  res.header('Access-Control-Allow-Headers', 'X-Requested-With, Content-Type')
  // 例如： 我们公司的请求头需要携带Authorization和Client-Type，此处就应该按照以下进行配置
  // res.header("Access-Control-Allow-Headers", "X-Requested-With, Content-Type, Authorization, Client-Type");
  next()
})

// 列表接口
app.get('/article/list', function (req, res) {
  res.json(Mock.mock(articleAPI.getList(req)))
})

app.listen('3000', () => {
  console.log('监听端口 3000')
})

```

article.js

```
let Mock = require('mockjs') // 引入mock
let param2Obj = require('./tool')

const List = []
const count = 100

for (let i = 0; i < count; i++) {
  List.push(Mock.mock({
    id: '@increment',
    timestamp: +Mock.Random.date('T'),
    author: '@first',
    reviewer: '@first',
    title: '@ctitle(5, 10)',
    content_short: '我是测试数据',
    content: '@csentence(10, 25)',
    forecast: '@float(0, 100, 2, 2)',
    importance: '@integer(1, 3)',
    'type|1': ['CN', 'US', 'JP', 'EU'],
    'status|1': ['published', 'draft', 'deleted'],
    display_time: '@datetime',
    comment_disabled: true,
    pageviews: '@integer(300, 5000)',
    image_uri: "@image('200x200','red', '#fff', '图片' )",
    platforms: ['a-platform']
  }))
}

module.exports = {
  getList: config => {
    const { importance, type, title, page = 1, limit = 20, sort } = param2Obj(config.url)

    let mockList = List.filter(item => {
      if (importance && item.importance !== +importance) return false
      if (type && item.type !== type) return false
      if (title && item.title.indexOf(title) < 0) return false
      return true
    })

    if (sort === '-id') {
      mockList = mockList.reverse()
    }

    const pageList = mockList.filter((item, index) => index < limit * page && index >= limit * (page - 1))

    return {
      total: mockList.length,
      items: pageList
    }
  },
  getPv: () => ({
    pvData: [{ key: 'PC', pv: 1024 }, { key: 'mobile', pv: 1024 }, { key: 'ios', pv: 1024 }, { key: 'android', pv: 1024 }]
  }),
  getArticle: (config) => {
    const { id } = param2Obj(config.url)
    for (const article of List) {
      if (article.id === +id) {
        return article
      }
    }
  },
  createArticle: () => ({
    data: 'success'
  }),
  updateArticle: () => ({
    data: 'success'
  })
}


```
[参考链接](https://juejin.im/post/5bf23603e51d452cf56d2902)

> tips:
> express + mock 用node.js 起一个本地服务接口
和第一种方式的区别是文件的导入方式由import换成了require


### javascript中的require、import和export

#### require时代
在现有的运行环境中，实现”模块”的效果，
##### 1. 原始写法
模块就是实现特定功能的一组方法。
```
function m2(){
　//...　　
}
```

##### 2. 对象写法
可以把模块写成一个对象，所有的模块成员都放到这个对象里面

```
var module1 = new Object({
_count : 0,
　m1 : function (){
　　//...
　},
　m2 : function (){
　　//...
　}
});

```
```
使用的时候，就是调用这个对象的属性
module1.m1();
module._count = 1;
```
写法会==暴露所有模块成员==，==内部状态可以被外部改写==。比如，外部代码可以直接改变内部计数器的值

##### 3. 立即执行函数写法
”立即执行函数”（Immediately-Invoked Function Expression，IIFE），可以达到==不暴露私有成员==

```
var module = (function() {
    var _count = 0;
    var m1 = function() {
        alert(_count)
    }
    var m2 = function() {
        alert(_count + 1)
    }

    return {
        m1: m1,
        m2: m2
    }
})()

// 外部代码无法读取内部的_count变量。
```

CommonJS中，有一个全局性方法require()，用于加载模块。
才有了后面的AMD、CMD 也采用的require方式来引用模块的风格

> 服务器端模块以后，大家就想要客户端模块。而且最好两者能够兼容，一个模块不用修改，==在服务器和浏览器都可以运行==。

但是，由于一个重大的局限，使得CommonJS规范不适用于浏览器环境。还是上一节的代码，如果在浏览器中运行，会有一个很大的问题
```
var math = require('math');
math.add(2, 3);
```
第二行math.add(2, 3)，在第一行require(‘math’)之后运行，因此必须等math.js加载完成。也就是说，如果加载时间很长，整个应用就会停在那里等。

服务器端: 模块都存放在本地硬盘，可以同步加载完成，待时间就是硬盘的读取时间

客户端：模块都放在服务器端，等待时间取决于网速的快慢，可能要等很长时间，浏览器处于”假死”状态。因此，浏览器端的模块，不能采用”同步加载”（synchronous），只能采用”异步加载”（asynchronous）。这就是AMD规范诞生的背景。

###  AMD是”Asynchronous Module Definition”
==模块==必须采用特定的==define()函数==来定义

模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。
#### 1. 定义模块
```

define(id?, dependencies?, factory)

//id:字符串，模块名称(可选)

//dependencies: 是我们要载入的依赖模块(可选)，使用相对路径。,注意是数组格式

//factory: 工厂方法，返回一个模块函数
```
1. 不依赖模块，直接定义在define（）
```
define(
    function (){
　　　　var add = function (x,y){
　　　　　　return x+y;
　　　　};
　　　　return {
　　　　　　add: add
　　　　};
　　}
　);
```
2. 依赖其他模块
```
define(['Lib'], function(Lib){
　　　　function foo(){
　　　　　　Lib.doSomething();
　　　　}
　　　　return {
　　　　　　foo : foo
　　　　};
　　}
　);
```
#### 2. 加载模块

```
require([module], callback);

例如：
require(['math'], function (math) {
　math.add(2, 3);
});

```
AMD比较适合浏览器环境。主要有两个Javascript库实现了AMD规范：require.js和curl.js。

### CMD规范 （Common Module Definition）

也是采用特定的define()函数来定义,用require方式来引用模块,CMD则是依赖就近，用的时候再require。代表==seajs==

```
define(function(require, exports, module) {
    var clock = require('clock');
    clock.start();
});
```
### CMD与AMD区别
二者==皆为异步加载模块==。最大的区别是==对依赖模块的执行时机处理不同==，而不是加载的时机或者方式不同，

AMD依赖前置，js可以方便知道依赖模块是谁，立即加载；

CMD就近依赖，需要使用把模块变为字符串解析一遍才知道依赖了那些模块，这也是很多人诟病CMD的一点，牺牲性能来带来开发的便利性，实际上解析模块用的时间短到可以忽略。

### ES6标准
>export指令导出接口，以import引入模块

==node模块==中我们依然采用的是==CommonJS规范==，使用r==equire==引入模块，使用==module.exports==导出接口。

#### export导出模块/ import引入模块
> 模块导出时，可以指定模块的导出成员。导出成员可以认为是类中的公有对象，而非导出成员可以认为是类中的私有对象：

命名式导入（名称导入）和默认导入（定义式导入）

```
// 导出
// d.js
export default function() {}

// 等效于：
function a() {};
export {a as default};

// 导入
import a from './d';

// 等效于，或者说就是下面这种写法的简写，是同一个意思
import {default as a} from './d';

```

1.每个js文件一创建，都有一个var exports = module.exports = {},使exports和module.exports都指向一个空对象。
2. module.exports和exports所指向的内存地址相同