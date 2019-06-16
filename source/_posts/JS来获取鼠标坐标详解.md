---
title: JS来获取鼠标坐标详解
date: 2019-06-16 18:12:17
tags: javascript
---
## js 获取鼠标坐标方法：

- e.clientX/Y
- e.pageX/Y
- e.offsetX/Y
- e.layerX/Y
- e.screenX/Y

- **clientX/clientY**

    获取到的是触发点相对浏览器可视区域左上角距离，不随页面滚动而改变(不包含**==工具栏==、==导航栏==**等等）

    兼容性：所有浏览器均支持

- **screenX screenY**

    是用来获取鼠标点击位置到屏幕显示器的距离，距离的最大值需根据**屏幕分辨率的尺寸**来计算。不随页面滚动而改变

    兼容性：所有游览器都支持此属性。

- **pageX pageY**
    > page 和 client 类似，原点在屏幕左上角，client之浏览器显示区域，文档类容一屏装不下时，会出现滚动条，此时
    pageY = clientY + document.documentElement.scrollTop(滚动条高度);

    鼠标距离页面初始page原点的长度。

- **offsetX/offsetY**

    获取到是==触发点相对被触发dom的左上角距离==，不过左上角基准点在不同浏览器中有区别，其中在IE中以内容区左上角为基准点不包括边框，如果触发点在边框上会返回负值，而chrome中以边框左上角为基准点

    兼容性：IE所有版本，chrome，Safari均完美支持，Firefox不支持

- **layerX/layerY**

    获取到的是==触发点相对被触发dom左上角的距离==，数值与offsetX/Y相同，这个变量就是==firefox用来替代offsetX/Y的==，基准点为边框左上角，但是有个条件就是，==被触发的dom需要设置为position:relative或者position:absolute==，否则会返回相对html文档区域左上角的距离。

    兼容性：IE6/7/8不支持，opera不支持，IE9/10和Chrome、Safari均支持

![](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/A216DC35B5CB4E709DBC0CEF57931594/2354)

黑色div设置position:relative, margin:100px,
对蓝色div绑定事件，宽高50px
> screenX: 取值范围【200px ～ 250px】
>
> screenY: 取值范围【150px ～ 200px】+ 浏览器顶部导航高度
---
> clientX: 取值范围【200px ～ 250px】
>
> clientY: 取值范围【150px ～ 200px】
---
> page 和client 类似 ，垂直方向多了滚动条
> pageX: 取值范围【200px ～ 250px】
>
>==不存在滚动条时：== pageY: 取值范围【50px ～ 100px 和client 一样
>
> ==存在滚动条时：== pageY: 取值范围【50px ～ 100px】+ document.documentElement.scrollTop(滚动条高度)
---
> 被触发dom的左上角距离蓝色盒子左上角为原点（0，0）
> offsetX: 取值范围【0px ～ 50px】
>
> offsetY: 取值范围【0px ～ 50px】
---
> layer 黑色盒子设置相对行为position:relative，layer 取值相对黑色盒子
> layerX: 取值范围【100px ～ 150px】
>
> layerY: 取值范围【50px ～ 100px】



```
测试数据
screenX:229 —— screenY:313
clientX:229 —— clientY:179
layerX:129 —— layerY:79
offsetX:30 —— offsetY:29
pageX:229 —— pageY:179
```
> ==Tips:== 外界一个显示器使用两个显示器时，当把浏览器在左边副显示器时上，screenX为负值， 相对于右边的主显示器左上角为原点
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    *{
      margin: 0;
    }
    body{
      height: 1900px;
    }
    .box{
      margin-top: 8px;
      margin-left: 8px;
      width: 400px;
      height: 200px;
      background: #123;
      position: relative;
    }
    .i{
      width: 20px;
      height: 20px;
      margin-left: 100px;
      background: rgb(11, 127, 243);
    }
  </style>
</head>
<body>
  <div class="box">
    <div class="i"></div>
  </div>
</body>
<script>
  let element = document.querySelector('.i')
  element.addEventListener('mousedown', (e) => {
      console.log('screenX:' + e.screenX + ' —— ' + 'screenY:' + e.screenY)
      console.log('clientX:' + e.clientX + ' —— ' + 'clientY:' + e.clientY)
      console.log('layerX:' + e.layerX + ' —— ' + 'layerY:' + e.layerY)
      console.log('offsetX:' + e.offsetX + ' —— ' + 'offsetY:' + e.offsetY)
      console.log('pageX:' + e.pageX + ' —— ' + 'pageY:' + e.pageY)
      console.log('——————————————————')
    })
</script>
</html>
```
### 参考链接
[1. https://segmentfault.com/a/1190000002405897](https://segmentfault.com/a/1190000002405897)