---
title: js 隐含参数
date: 2018-11-04 20:59:53
tags: argument 
---

## arguments
> arguments 该对象代表正在执行的函数和调用它的函数的参数,Arguments是一个类似数组但不是数组的对象，说它类似数组是因为其具有数组一样的访问性质及方式，可以由arguments[n]来访问对应的单个参数的值，并拥有数组长度属性length,arguments对象存储的是实际传递给函数的参数，而不局限于函数声明所定义的参数列表，而且不能显式创建 arguments对象。arguments对象只有函数开始时才可用

判断arguments 是对象 不死数组

```
function a (a){alert(arguments instanceof Array);}


function b (a){alert(arguments instanceof Object);}

a() //  false
b() //  true


Array.prototype.selfvalue = 1;
alert(new Array().selfvalue); //  1

function testAguments(){
  alert(arguments.selfvalue);// undefined
}
```

## caller

> 返回一个对函数的引用，该函数调用了当前函数。

## callee

> 返回正被执行的 Function 对象，也就是所指定的 Function 对象的正文
> callee属性是 arguments 对象的一个成员，它表示对函数对象本身的引用，有利于匿名函数的递归或者保证函数的封装性
> arguments.length是实参长度，arguments.callee.length是形参长度

```
//递归计算
var sum = function(n){
if (n <= 0) 
return 1;
else
return n + arguments.callee(n - 1)
```

## apply and call 

> 它们的作用都是将函数绑定到另外一个对象上去运行
> 可实现将函数作为另外一个对象的方法运行的目的

```
apply(thisArg,argArray); // argArray不是一个有效的数组或者不是 arguments对象，那么将导致一个 TypeError。

call(thisArg[,arg1,arg2…] ]);

所有函数内部的this指针都会被赋值为thisArg

没有提供 thisArg参数，那么 Global 对象被用作 thisArg
```

**tips:**   用call和apply应用另一个函数（类）以后，当前的函数（类）就具备了另一个函数（类）的方法或者是属性，这也可以称之为“继承”

```
var vehicle=Class.create();
vehicle.prototype={
  initialize:function(type){
    this.type=type;
  }
  showSelf:function(){
    alert("this vehicle is "+ this.type);
  }
}
var moto=new vehicle("Moto");
moto.showSelf();
```