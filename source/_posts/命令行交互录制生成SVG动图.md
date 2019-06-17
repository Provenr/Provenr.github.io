---
title: 命令行交互录制生成SVG动图
date: 2019-05-31 16:05:46
tags: [asciinema,命令行录制]
---
# 前言
配置服务器时，操作命令比较多，复杂，为了完整记录你在终端中执行的操作，将其保存以供后续参考学习，发现一个神器[asciinema](https://github.com/asciinema/asciinema)
将终端会话录制成 SVG 动画
## 1.  asciinema的使用
### asciinema的安装
因为使用的Mac系统 安装使用brew，安装命令如下：
```

brew install asciinema

```

### asciinema使用

```
 asciinema rec XXXX/XX.json

# 录制屏幕数据放在 Desktop目录下的 centos.json文件中
 asciinema rec Desktop/centos.json


 #  (choose from 'rec', 'play', 'cat', 'upload', 'auth')
```
- ####  rec [filename]
>  记录终端会话
- ####  play [filename]
>  回放记录asciicast终端

1. 开启记录
```
asciinema rec
```
2. 结束记录
```
<ctrl-d> 结束记录
```
3.结束记录
```
<enter> 上传到 asciinema.org,
<ctrl-c> 存到本地，如果上传 生成一个链接 URL
```
4.回放记录
```
asciinema play <URL>

# 回放记录操作

#  Space（空格） - 暂停/播放
#  <.> 点操作 - 暂停时 可以步进
#  <Ctrl+C> - 退出.
```

- ####  cat <filename>
>  打印完整记录asciicast的输出终端。


### 结束录制
Ctrl + D 结束录制，XX.json文件就保存了我们的命令行交互记录。

## 2. 数据转SVG
### [svg-term-cli](https://www.npmjs.com/package/svg-term-cli)
用于转换 XX.json文件中数据转化为 svg 格式动图
```
npm install -g svg-term-cli

 将命令转为svg 图片
cat Desktop/centos.json | svg-term --height=40 --out=Desktop/centos.svg --window
```
### 已经存在 录制好到json 文件 可以下面命令生成svg
```
svg-term --in Desktop/local.json --out Desktop/1.svg --height=50 --window
```
--in        json file to use as input
--width     列宽 [number]
--height    height  [number]
--window    用窗口渲染


### 参考链接
【1】https://blog.csdn.net/u014628388/article/details/81085635
【2】https://github.com/asciinema/asciinema
