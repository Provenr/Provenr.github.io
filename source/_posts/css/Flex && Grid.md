---
title: flex && grid 布局
date: 20198-09-25
tags: [flex, grid]
categories: css 
---


![](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/1FA696763A6A4622B6F59B6B86862EED/4534)

# 引言
Flex 是一维布局方式，我们需要不断嵌套 Div 才能形成复杂结构，一旦布局产生了变化时难以维护，代码就需要重构，Grid是二维布局，即便布局方式做了翻天覆地的调整，也仅需少量修改就能适配。详细理解参考后面例子

# 概述
### flex 局限性
1. Flex 在复杂页面时的Div层级复杂，而一旦布局产生了变化时难以维护，代码就需要重构

2. Flex 布局有一些不受控制的智能设定,宽度 50% 的子元素会被同级元素挤到 50% 以下,由于没有提供像 Grid 的 minmax 之类的 API，所以定制型不足

3. 详细flex使用方法参考[flex布局](https://juejin.im/post/58e3a5a0a0bb9f0069fc16bb)

## 举个例子：
![](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/0B30BEE593DC4EF58E82E0F0E88B9B59/4566)
#### 1.使用flex画出上面的页面结构
```
<div class="card">
  <div class="profile-sidebar">
    <img src="https://i.pravatar.cc/125?image=3" alt="" class="profile-img">
    <ul class="social-list">
      <li><a href="#" class="social-link"><i class="fab fa-dribbble-square"></i></a></li>
      <li><a href="#" class="social-link"><i class="fab fa-facebook-square"></i></a></li>
      <li><a href="#" class="social-link"><i class="fab fa-twitter-square"></i></a></li>
    </ul>
  </div>
  <div class="profile-body">
    <h2 class="profile-name">Ramsey Harper</h2>
    <p class="profile-position">Graphic Designer</p>
    <p class="profile-info">Lorem ipsum dolor sit amet consectetur adipisicing elit. Facere a tempore, dignissimos odit accusantium repellat quidem, sit molestias dolorum placeat quas debitis ipsum esse rerum?</p>
  </div>
</div>
```
> 样式使用了响应式，在页面最小宽度大于等于450px事，才呈现和例子一样的效果

```scss
.card {
    box-shadow: 0 0 20px rgba(0,0,0,.2);
    width: 80%;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    max-width: 600px;
    background: #005E9B;
    flex-basis: 250px;
    color: white;
    padding: 2em;
    text-align: center;
}
.profile-info {
    font-weight: 300;
    opacity: .7;
}
.profile-sidebar {
    margin-right: 2em;
    text-align: center;
}
.profile-name {
    letter-spacing: 1px;
    font-size: 2rem;
    margin: .75em 0 0;
    line-height: 1;
}

.profile-name::after {
    content: '';
    display: block;
    width: 2em;
    height: 1px;
    background: #5bcbf0;
    margin: .5em auto .65em;
    opacity: .25;
}

.profile-position {
    text-transform: uppercase;
    font-size: .875rem;
    letter-spacing: 3px;
    margin: 0 0 2em;
    line-height: 1;
    color: #5bcbf0;
}

.profile-img {
    max-width: 100%;
    border-radius: 50%;
    border: 2px solid white;
}

.social-list {
    list-style: none;
    justify-content: space-evenly;
    display: flex;
    min-width: 125px;
    max-width: 175px;
    margin: 0 auto;
    padding: 0;
}

.social-link {
    color: #5bcbf0;
    opacity: .5;
}

.social-link:hover,
.social-link:focus {
    opacity: 1;
}
        
@media (min-width: 450px) {
    .card {
        flex-direction: row;
        text-align: left;
    }

    .profile-name::after {
        margin-left: 0;
    }
}

```
在使用flex 布局时 需要自己创建内容列（图中所示的黄色和橘黄色圈起来的部分），需要额外的Div标记，使得DOM结构变得复杂

#### 2.grid 布局

```
<div class="card">
    <img src="https://i.pravatar.cc/125?image=3" alt="" class="profile-img">
    <ul class="social-list">
        <li><a href="#" class="social-link"><i class="fab fa-dribbble-square">AA</i></a></li>
        <li><a href="#" class="social-link"><i class="fab fa-facebook-square">BB</i></a></li>
        <li><a href="#" class="social-link"><i class="fab fa-twitter-square">CC</i></a></li>
    </ul>
    <h2 class="profile-name">Ramsey Harper</h2>
    <p class="profile-position">Graphic Designer</p>
    <p class="profile-info">Lorem ipsum dolor sit amet consectetur adipisicing elit. Facere a tempore, dignissimos odit accusantium repellat quidem, sit molestias dolorum placeat quas debitis ipsum esse rerum?</p>
</div>

```
css 只有核心grid样式，其他辅助样式参考上面flex布局的
```scss
 .card {
    box-shadow: 0 0 20px rgba(0,0,0,.2);
    width: 80%;
    margin: 0 auto;
    max-width: 600px;
    background: #005E9B;
    color: white;
    padding: 2em;
    text-align: left;

    display: grid;
    grid-template-columns: 1fr 3fr;
    grid-column-gap: 2em;
    grid-template-areas:
            "image name"
            "image position"
            "social description";
}

.profile-name     { grid-area: name; }
.profile-position { grid-area: position; }
.profile-info     { grid-area: description; }
.profile-img      { grid-area: image; }
.social-list      { grid-area: social; }

```
因为grid是二维的，这意味着它允许我们同时创建行和列，我们的网格容器可以完全控制其内部布局。提供一种更直观的描述 UI 的方式
grid-template-areas将dom抽象的表示在二维网格中，这种描述方式适配不同分辨率下（响应式布局）也具有优势

### 如果上面设计图变更为下面图所示
![](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/C8B2BE3F41064954A55777B4FF58320D/4669)
使用flex布局，不更改DOM结构就无法做到上面设计稿的布局
使用Grid布局只需要将网格模板区域修改为以下内容，就能轻松搞定

```
.card{
    grid-template-areas:
      "image name"
      "image position"
      "image social"
      ". description";
}
```
### Grid带来的响应式便利

```
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            font-family: 'Fira Sans', sans-serif;
            line-height: 1.6;
            min-height: 100vh;
            display: grid;
            place-items: center;
        }
    </style>
    <style>


        .card {
        // box-shadow: 0 0 20px rgba(0,0,0,.2);
            width: 80%;
            margin: 0 auto;
            display: flex;
            flex-direction: column;
            max-width: 600px;
            background: #005E9B;
            flex-basis: 250px;
            color: white;
            padding: 2em;
            text-align: center;

            display: grid;
            grid-template-columns: 1fr;
            grid-column-gap: 2em;
            grid-template-areas:
                    "name"
                    "image"
                    "social"
                    "position"
                    "description";
        }


        .profile-name     { grid-area: name; }
        .profile-position { grid-area: position; }
        .profile-info     { grid-area: description; }
        .profile-img      { grid-area: image; }
        .social-list      { grid-area: social; }



        .profile-info {
            font-weight: 300;
            opacity: .7;
        }

        .profile-name {
            letter-spacing: 1px;
            font-size: 2rem;
            margin: .75em 0 0;
            line-height: 1;
        }

        .profile-name::after {
            content: '';
            display: block;
            width: 2em;
            height: 1px;
            background: #5bcbf0;
            margin: .5em auto .65em;
            opacity: .25;
        }

        .profile-position {
            text-transform: uppercase;
            font-size: .875rem;
            letter-spacing: 3px;
            margin: 0 0 .5em;
            line-height: 1;
            color: #5bcbf0;
        }

        .profile-img {
            justify-self: center;
            max-width: 100%;
            border-radius: 50%;
            border: 2px solid white;
        }

        .social-list {
            justify-self: center;
            display: flex;
            list-style: none;
            justify-content: space-between;
            width: 75px;
            margin: .5em 0 0;
            padding: 0;
        }

        .social-link {
            color: #5bcbf0;
            opacity: .5;
        }

        .social-link:hover,
        .social-link:focus {
            opacity: 1;
        }
        @media (min-width: 600px) {
            .card {
                text-align: left;
                grid-template-columns: 1fr 3fr;
                grid-template-areas:
                        "image name"
                        "image position"
                        "social description";
            }

            .profile-name::after {
                margin-left: 0;
            }
        }
    </style>

</head>
<body>
<div class="card">
    <img src="https://i.pravatar.cc/125?image=3" alt="" class="profile-img">
    <ul class="social-list">
        <li><a href="#" class="social-link"><i class="fab fa-dribbble-square">AA</i></a></li>
        <li><a href="#" class="social-link"><i class="fab fa-facebook-square">BB</i></a></li>
        <li><a href="#" class="social-link"><i class="fab fa-twitter-square">BB</i></a></li>
    </ul>
    <h2 class="profile-name">Ramsey Harper</h2>
    <p class="profile-position">Graphic Designer</p>
    <p class="profile-info">Lorem ipsum dolor sit amet consectetur adipisicing elit. Facere a tempore, dignissimos odit accusantium repellat quidem, sit molestias dolorum placeat quas debitis ipsum esse rerum?</p>
</div>

</body>
</html>

```
[在线查看](https://codepen.io/provenr/pen/BaapVGW)

## 补充 
#### 1. CSS [justify-content属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/justify-content)如何分配顺着父容器主轴的弹性元素之间及其周围的空间。
```
 - justify-content: space-between;  /* 均匀排列每个元素
 - justify-content:space-around; // 每行第一个元素到行首的距离和每行最后一个元素到行尾的距离将会是相邻元素之间距离的一半
 - justify-content: space-evenly; //均匀排列每个元素每个元素之间的间隔相等
```

#### 2. Grid兼容性
![](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/F21786626F68441B9CEF10810A107582/4531)
#### 3. 在 Grid 语法中只需要用 [ css grid generator](https://cssgrid-generator.netlify.com/)拖拖拽拽即可自动生成布局代码

## 总结
flexbox在创建布局方面给我们带来了非常奇妙的体验，grid有许多新的属性和值，比flexbox复杂，但是它为我们提供了更多的控制权。Flex 依然有使用场景，即简单的一维结构，或者 space-between 等 Flex 独有语法的情况。因此推荐整体、复杂的二维布局采用 Grid，一维的简单布局采用 Flex。

==grid-template-columns== ==grid-template-rows== 组合起来使用比 grid-template-areas 更强大，但是纯代码方式描述没有 **grid-template-areas更直观**

理解了这些也就理解了布局未来的发展方向，Grid让==布局与 Dom 分离== 一直是前端的一个梦想，开发 UI 部分时，==只需关心页面由哪些模块组成==，去实现这些模块就行了，==而不需要关心模块之间应该如何组合==。在描述组合时，可以通过可视化或比较抽象的字符串描述布局的结构，并对应到写好的模块上，这样的代码维护性远高于用 DIV 描述结构的方案


## 参考文档
【1】[CSS网格如何改变布局方式](https://www.freecodecamp.org/news/css-grid-changes-how-we-can-think-about-structuring-our-content/)
【2】[兼容性检测](https://caniuse.com/#search=grid-template-areas)
【3】[flex布局](https://juejin.im/post/58e3a5a0a0bb9f0069fc16bb)
【4】[精读《用 css grid 重新思考布局》](https://juejin.im/post/5da3cb19518825164d2cd91d)