---
title: 'js 生成[min,max]范围的 随机数'
date: 2019-07-04 23:42:19
tags: [随机数,javaScript]
---
[TOC]
# 预备知识
> 随机数生成 利用 Math.random()

1. Math.random();  //0.0 ~ 1.0 之间的一个伪随机数。【包含0不包含1】

2. Math.ceil();  //向上取整。

3. Math.floor();  //向下取整。

4. Math.round();  //四舍五入。

获取 [1, 10]的随机数
```
Math.ceil(Math.random()*10);      // 获取从1到10的随机整数 ，取0的概率极小。

Math.round(Math.random());   //可均衡获取0到1的随机整数。

Math.floor(Math.random()*10);  //可均衡获取0到9的随机整数。

Math.round(Math.random()*10);  //基本均衡获取0到10的随机整数，其中获取最小值0和最大值10的几率少一半。

```
> ==使用math.round()，其中获取最小值0和最大值10的几率少一半。因为结果在0~0.4 为0，0.5到1.4为1...8.5到9.4为9，9.5到9.9为10。所以头尾的分布区间只有其他数字的一半。==

# 生成[min,max]的随机数
## 过程分析
> Math.random()生成[0,1)的数，所以
>Math.random()*5生成{0,5)的数。
>
> 通常期望得到整数，所以要对得到的结果处理一下。
>
> parseInt()，Math.floor()，Math.ceil()和Math.round()都可得到整数。
>
> parseInt()和Math.floor()结果都是向下取整。
>
> 所以Math.random()*5生成的都是[0,4] 的随机整数。

## 所以生成[1,max]的随机数，公式如下：

```
Math.floor(Math.random() * max) + 1

```

## 所以生成[1,max]的随机数，公式如下：

```
Math.floor(Math.random()*(max+1));

```

## 所以希望生成[min,max]的随机数，公式如下：

```
Math.floor(Math.random()*(max-min+1)+min);
```
### 参考链接
【1】[生成[min,max]范围的 随机数](https://www.cnblogs.com/starof/p/4988516.html)