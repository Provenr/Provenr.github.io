---
title: js 滑屏操作反弹原理
date: 2019-06-16 23:53:28
tags: [javascript, touch]
---
[toc]
## 移动端事件

1. **touchstart事件**：当手指触摸屏幕时候触发，即使已经有一个手指放在屏幕上也会触发。
2.  **touchmove事件**：当手指在屏幕上滑动的时候连续地触发。在这个事件发生期间，调用preventDefault()事件可以阻止滚动。
3.  **touchend事件**：当手指从屏幕上离开的时候触发。
4.  **touchcancel事件**：当系统停止跟踪触摸的时候触发。关于这个事件的确切出发时间，文档中并没有具体说明，咱们只能去猜测了。
#### 触摸事件的属性。

- **touches**：表示当前跟踪的触摸操作的Touch对象的数组。(**当前位于屏幕**下的手指列表信息)

- **targetTouches**：特定于事件目标的Touch对象的数组。(当前位于**当前元素**下的手指列表信息)

- **changeTouches**：表示自上次触摸以来发生了什么改变的Touch对象的数组。(当前涉及到**当前事件**的手指列表)
###  每个Touch对象包含的属性

 - **clientX**：触摸目标在视口中的x坐标。
 - **clientY**：触摸目标在视口中的y坐标。
 - **identifier**：标识触摸的唯一ID。
 - **pageX**：触摸目标在页面中的x坐标。
 - **pageY**：触摸目标在页面中的y坐标。
 - **screenX**：触摸目标在屏幕中的x坐标。
 - **screenY**：触摸目标在屏幕中的y坐标。
 - **target**：触目的DOM节点目标。

### 阻止默认事件
```
 document.addEventListener('touchstart',function(e){
    e.preventDefault();
  },{
    passive:false
  });
```
>Tips: 阻止所有点击
根据实际需要重新开启交互行为
```
a.ontouchstart = function(){
    window.location = this.href;
}
```
- 能够阻止IOS10缩放：
- 对于ISO10设置meta标签禁止缩放是没有作用的，上面的代码阻止了浏览器的默认行为。
- 阻止IOS10下回弹效果
- 去除系统滚动条
- 禁止长按选中文字和图片：当然**也同时阻止了input获取焦点的行为，这就需要使用单独为input添加一个阻止冒泡的行为。以免事件冒泡至顶层元素而被阻止交互行为**。
```
input.ontouchstart = function(ev){
      ev.stopPropagation();
}
```
### 事件监听**addEventListener()**
```
addEventListener(type, listener[, useCapture ])
addEventListener(type, listener[, options ])


```
- useCapture: 控制监听器是在捕获阶段执行还是在冒泡阶段执行的 useCapture 参数
- options 对象可用的属性有三个：
```
addEventListener(type, listener, {
    capture: false,
    passive: false,
    once: false
})
三个属性都是布尔类型的开关，默认值都为 false
```
 -  **capture** 属性等价于以前的 useCapture 参数
 -  **once** 属性就是表明该监听器是一次性的，执行一次后就被自动 removeEventListener 掉，==还没有浏览器实现它==
 -  **passive** 属性是本文的主角，Firefox 和 Chrome 已经实现

####  **==passive==**

==**场景**：== 移动端的页面都会监听 touchstart 等 touch 事件，由于 touchstart 事件对象的 cancelable 属性为 true, ==它的默认行为通常来说就是滚动当前页面（还可能是缩放页面）==

==影响：== 如果它的默认行为被阻止了，页面就必须静止不动。但浏览器无法预先知道一个监听器会不会调用 preventDefault()，它能做的只有等监听器执行完后再去执行默认行为，而监听器执行是要耗时的，有些甚至耗时很明显，这样就会导致页面卡顿。视频里也说了，即便监听器是个空函数，也会产生一定的卡顿，毕竟空函数的执行也会耗时。

> passive 的意思是“顺从的”，表示它不会对事件的默认行为说 no，浏览器知道了一个监听器是 passive 的，它就可以在两个线程里同时执行监听器中的 JavaScript 代码和浏览器的默认行为了。
```
document.addEventListener("touchstart", function(e){
 e.preventDefault() //通过 preventDefault() 方法阻止默认行为
})

```

----
## 参考链接
【1】[移动端事件详解](https://juejin.im/post/5a996388f265da2388)
【2】[passive 解读](https://www.cnblogs.com/ziyunfei/p/5545439.html)
##  滑屏操作例子
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
  #view{
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;

  }
  #list{
    position: absolute;
    width: 2000px;
    left: 0;top: 0;
    margin: 0;padding: 0;
  }
  #list li{
    float: left;
    width: 100px;
    height: 100%;
    box-sizing: border-box;
    border: 1px solid #123;
    list-style: none;
    font: 50px/200px '宋体';
    text-align: center;
  }
  </style>
</head>
<body>
<div id="view">
  <ul id="list">

  </ul>
</div>
<script src="js/mTween.js"></script>
<script>
  document.addEventListener('touchstart',function(e){
    e.preventDefault();
  },{
    passive:false
  });

(function(){
  var list = document.querySelector('#list');
  var inner = '';
  for( var i = 0; i < 20; i++){
    inner += `<li>${i}</li>`
  }
  list.innerHTML = inner

  var startPoint = 0;
  var startPosition = 0;
  var screenWidth = window.innerWidth; // 屏幕宽度
  var listWidth = document.querySelector('#list').offsetWidth //列表宽度
  list.addEventListener('touchstart', function(e){
    startPoint = e.changedTouches[0].pageX;
    startPosition = css(list,'left')
    console.log(startPoint)
  })
  list.addEventListener('touchmove', function(e){
    var newPoint = e.changedTouches[0].pageX;
    var dis = newPoint - startPoint; // 终止点与起始点距离
    var moveDis = dis + startPosition // 列表移动距离
    // 往右移动 超过半屏  移动距离设置为屏幕宽度一半
    if(moveDis > screenWidth / 2) {
      moveDis = screenWidth / 2
    }
    // 往左移动 最多移动距离为 列表宽度 - 屏幕宽度
    // 列表尾部 往左移动超过半屏  移动距离（绝对值）设置为 列表宽度 - 屏幕宽度 + 屏幕宽度一半
    listWidth - screenWidth + screenWidth / 2
    if(Math.abs(moveDis) > Math.abs(listWidth - screenWidth + screenWidth / 2)) {
      moveDis = - (Math.abs(listWidth - screenWidth + screenWidth / 2)) // 设置为往左移动最大距离
    }
    css(list,'left',moveDis)
  })
  list.addEventListener('touchend', function(e){
    var newPoint = e.changedTouches[0].pageX;
    var dis = newPoint - startPoint; // 终止点与起始点距离
    var moveDis = dis + startPosition // 列表移动距离
    // 往右滑动临界值
    if(moveDis > 0){
      mTween({
          //运动元素
          el:list,
          //运动样式
          attrs:{
              left:0,
          },
          duration:1000
      });
    }
    // 往左滑动临界值
    if(Math.abs(moveDis) > Math.abs(listWidth - screenWidth)){
      mTween({
          //运动元素
          el:list,
          //运动样式
          attrs:{
              left:- Math.abs(listWidth - screenWidth),
          },
          duration:1000
      });
    }
  })
})()
</script>

</body>
</html>

```

### 附录
```
var normalAttr = [
  'width',
  'height',
  'left',
  'top',
  'bottom',
  'right',
  'marginLeft',
  'marginTop',
  'marginBottom',
  'marginRight'
];

var css3Attr = [
  'rotate',
  'rotateX',
  'rotateY',
  'skewX',
  'skewY',
  'translateX',
  'translateY',
  'translateZ',
  'scale',
  'scaleX',
  'scaleY'
];
var Tween = {
	linear: function (t, b, c, d){  //匀速
		return c*t/d + b;
	},
	easeIn: function(t, b, c, d){  //加速曲线
		return c*(t/=d)*t + b;
	},
	easeOut: function(t, b, c, d){  //减速曲线
		return -c *(t/=d)*(t-2) + b;
	},
	easeBoth: function(t, b, c, d){  //加速减速曲线
		if ((t/=d/2) < 1) {
			return c/2*t*t + b;
		}
		return -c/2 * ((--t)*(t-2) - 1) + b;
	},
	easeInStrong: function(t, b, c, d){  //加加速曲线
		return c*(t/=d)*t*t*t + b;
	},
	easeOutStrong: function(t, b, c, d){  //减减速曲线
		return -c * ((t=t/d-1)*t*t*t - 1) + b;
	},
	easeBothStrong: function(t, b, c, d){  //加加速减减速曲线
		if ((t/=d/2) < 1) {
			return c/2*t*t*t*t + b;
		}
		return -c/2 * ((t-=2)*t*t*t - 2) + b;
	},
	elasticIn: function(t, b, c, d, a, p){  //正弦衰减曲线（弹动渐入）
		if (t === 0) {
			return b;
		}
		if ( (t /= d) == 1 ) {
			return b+c;
		}
		if (!p) {
			p=d*0.3;
		}
		if (!a || a < Math.abs(c)) {
			a = c;
			var s = p/4;
		} else {
			var s = p/(2*Math.PI) * Math.asin (c/a);
		}
		return -(a*Math.pow(2,10*(t-=1)) * Math.sin( (t*d-s)*(2*Math.PI)/p )) + b;
	},
	elasticOut: function(t, b, c, d, a, p){    //*正弦增强曲线（弹动渐出）
		if (t === 0) {
			return b;
		}
		if ( (t /= d) == 1 ) {
			return b+c;
		}
		if (!p) {
			p=d*0.3;
		}
		if (!a || a < Math.abs(c)) {
			a = c;
			var s = p / 4;
		} else {
			var s = p/(2*Math.PI) * Math.asin (c/a);
		}
		return a*Math.pow(2,-10*t) * Math.sin( (t*d-s)*(2*Math.PI)/p ) + c + b;
	},
	elasticBoth: function(t, b, c, d, a, p){
		if (t === 0) {
			return b;
		}
		if ( (t /= d/2) == 2 ) {
			return b+c;
		}
		if (!p) {
			p = d*(0.3*1.5);
		}
		if ( !a || a < Math.abs(c) ) {
			a = c;
			var s = p/4;
		}
		else {
			var s = p/(2*Math.PI) * Math.asin (c/a);
		}
		if (t < 1) {
			return - 0.5*(a*Math.pow(2,10*(t-=1)) *
					Math.sin( (t*d-s)*(2*Math.PI)/p )) + b;
		}
		return a*Math.pow(2,-10*(t-=1)) *
				Math.sin( (t*d-s)*(2*Math.PI)/p )*0.5 + c + b;
	},
	backIn: function(t, b, c, d, s){     //回退加速（回退渐入）
		if (typeof s == 'undefined') {
		   s = 1.70158;
		}
		return c*(t/=d)*t*((s+1)*t - s) + b;
	},
	backOut: function(t, b, c, d, s){
		if (typeof s == 'undefined') {
			s = 1.70158;  //回缩的距离
		}
		return c*((t=t/d-1)*t*((s+1)*t + s) + 1) + b;
	},
	backBoth: function(t, b, c, d, s){
		if (typeof s == 'undefined') {
			s = 1.70158;
		}
		if ((t /= d/2 ) < 1) {
			return c/2*(t*t*(((s*=(1.525))+1)*t - s)) + b;
		}
		return c/2*((t-=2)*t*(((s*=(1.525))+1)*t + s) + 2) + b;
	},
	bounceIn: function(t, b, c, d){    //弹球减振（弹球渐出）
		return c - Tween['bounceOut'](d-t, 0, c, d) + b;
	},
	bounceOut: function(t, b, c, d){//*
		if ((t/=d) < (1/2.75)) {
			return c*(7.5625*t*t) + b;
		} else if (t < (2/2.75)) {
			return c*(7.5625*(t-=(1.5/2.75))*t + 0.75) + b;
		} else if (t < (2.5/2.75)) {
			return c*(7.5625*(t-=(2.25/2.75))*t + 0.9375) + b;
		}
		return c*(7.5625*(t-=(2.625/2.75))*t + 0.984375) + b;
	},
	bounceBoth: function(t, b, c, d){
		if (t < d/2) {
			return Tween['bounceIn'](t*2, 0, c, d) * 0.5 + b;
		}
		return Tween['bounceOut'](t*2-d, 0, c, d) * 0.5 + c*0.5 + b;
	}
}
function css(ele, attr, val){
  if(typeof attr === 'string' && typeof val === 'undefined'){
    if(css3Attr.indexOf(attr) !== -1){
      return transform(ele, attr);
    }
    var ret = getComputedStyle(ele)[attr];
    return normalAttr.indexOf(attr) !== -1 ? parseFloat(ret) : ret * 1 === ret * 1 ? ret*1 : ret;
  }

  function setAttr(attr, val){
    if(css3Attr.indexOf(attr) !== -1){
      return transform(ele, attr, val);
    }
    if(normalAttr.indexOf(attr) !== -1){
      ele.style[attr] = val ? val + 'px' : val;
    }else{
      ele.style[attr] = val;
    }
  }

  // 批量设置
  if(typeof attr === 'object'){
    for(var key in attr){
      setAttr(key, attr[key]);
    }
    return;
  }

  setAttr(attr, val);
}

function transform(el, attr, val){
  el._transform = el._transform || {};

  if(typeof val === 'undefined'){
    return el._transform[attr];
  }

  el._transform[attr] = val;

  var str = '';

  for(var key in el._transform){
    var value = el._transform[key];
    switch (key) {
      case 'translateX':
      case 'translateY':
			case 'translateZ':
        str += `${key}(${value}px) `;
        break;
      case 'rotate':
      case 'rotateX':
      case 'rotateY':
      case 'skewX':
      case 'skewY':
        str += `${key}(${value}deg) `;
        break;
      default:
        str += `${key}(${value}) `;
    }
  }

  el.style.transform = str.trim();
}

function mTween(props){
  var el = props.el;
	if(el.mTween) return;
  var duration = props.duration || 400,
      fx = props.fx || 'easeOut',
      cb = props.cb,
			attrs = props.attrs || {};
			s = props.s
  var beginData = {}, changeData = {};
	var maxDis = 0;
  for(var key in attrs){
    beginData[key] = css(el, key);
		changeData[key] = attrs[key] - beginData[key];
		maxDis = Math.max(Math.abs(changeData[key]),maxDis);
	}
	if(typeof duration == "object"){
		durationPorps = duration;
		duration = durationPorps.multiple!==undefined?maxDis*durationPorps.multiple:maxDis*1.2;
		if(durationPorps.min !== undefined){
			duration = duration < durationPorps.min?durationPorps.min:duration;
		}
		if(durationPorps.max !== undefined){
			duration = duration > durationPorps.max?durationPorps.max:duration;
		}

	}

  var startTime = Date.now();

  (function startMove(){
    el.mTween = window.requestAnimationFrame(startMove);
    var time = Date.now() - startTime;
    if(time > duration){
      time = duration;
      window.cancelAnimationFrame(el.mTween);
      el.mTween = null;
    }

    for(var key in attrs){
      var currentPos = Tween[fx](time, beginData[key], changeData[key], duration,s);
      css(el, key, currentPos);
    }

    if(time === duration && typeof cb === 'function'){
      cb.call(el);
    }
  })();
}

mTween.stop = function (el){
  window.cancelAnimationFrame(el.mTween);
  el.mTween = null;
};





```