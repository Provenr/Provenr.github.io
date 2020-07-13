---
title: docker 部署 Nginx
date: 2019-06-21 11:29:15
tags: [docker, Nginx]
---
[TOC]
# docker 部署 nginx
## 一、macOS 安装 Docker,windows安装Docker参见[Windows Docker 安装](https://www.runoob.com/docker/windows-docker-install.html)
### Homebrew 安装
```
brew cask install docker

```
### 手动下载安装
手动[下载](https://download.docker.com/mac/stable/Docker.dmg)，如同 macOS 安装其它软件一样，鲸鱼图标拖拽到 Application 文件夹
### 运行
从应用中找到 Docker 图标并点击运行。
![](https://yeasy.gitbooks.io/docker_practice/install/_images/install-mac-apps.png)
右上角菜单栏看到多了一个鲸鱼图标，这个图标表明了 Docker 的运行状态。
![](https://yeasy.gitbooks.io/docker_practice/install/_images/install-mac-menubar.png)
第一次点击图标，可能会看到这个安装成功的界面，点击 "Got it!" 可以关闭这个窗口。
![](https://yeasy.gitbooks.io/docker_practice/install/_images/install-mac-success.png)
以后每次点击鲸鱼图标会弹出操作菜单。
![](https://yeasy.gitbooks.io/docker_practice/install/_images/install-mac-menu.png)

启动终端后，通过命令可以检查安装后的 Docker 版本。
```
$ docker --version

    Docker version 18.09.0, build 4d60db4

$ docker-compose --version

    docker-compose version 1.23.2, build 1110ad01

$ docker-machine --version

    docker-machine version 0.16.0, build 702c267f
```
如果 docker version、docker info 都正常的话, 开始构建一个 [Nginx 服务器](https://hub.docker.com/_/nginx/)
## 二、部署 nginx
### 1. 首先pull下载nginx镜像包
```
docker pull nginx

// 起一个服务
docker run -d -p 8090:80 --name ponhuWeb nginx

```
### 2. 列出已经下载下来的镜像
```
docker image ls
```
### 3. 删除本地镜像
```
docker image rm <镜像>
<镜像>可以用 镜像短 ID、镜像长 ID、镜像名 或者 镜像摘要。

MacBook-Pro  ~  docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              719cd2e3ed04        7 days ago          109MB

如果要删除nginx 镜像，可以执行以下命令，

1.利用短ID删除
docker image ls 默认列出的就已经是短 ID，一般取前3个字符，就可以区分别的镜像
docker image rm 719

2.用镜像名
docker image rm nginx
```
### 4. 查看nginx镜像里面配置文件的具体位置，只有找到镜像配置文件的路径，后面挂载文件和文件夹才能覆盖这些路径
- 终端的方式打开镜像容器
    ```
        docker run -i -t nginx /bin/bash

        各个参数解析：

            -t:在新容器内指定一个伪终端或终端
            -i:允许你对容器内的标准输入 (STDIN) 进行交互
            -d:让容器在后台运行
            -P:将容器内部使用的网络端口映射到我们使用的主机上
            可以通过 -p 参数来设置不一样的端口
    ```
- 输入exit退出容器的终端
- 查看目录
```
ls -l

输出以下目录

drwxr-xr-x   2 root root 4096 Jun 10 00:00 bin
drwxr-xr-x   2 root root 4096 Mar 28 09:12 boot
drwxr-xr-x   5 root root  360 Jun 18 09:30 dev
drwxr-xr-x   1 root root 4096 Jun 18 09:30 etc
drwxr-xr-x   2 root root 4096 Mar 28 09:12 home
drwxr-xr-x   1 root root 4096 Jun 10 00:00 lib
drwxr-xr-x   2 root root 4096 Jun 10 00:00 lib64
drwxr-xr-x   2 root root 4096 Jun 10 00:00 media
drwxr-xr-x   2 root root 4096 Jun 10 00:00 mnt
drwxr-xr-x   2 root root 4096 Jun 10 00:00 opt
dr-xr-xr-x 202 root root    0 Jun 18 09:30 proc
drwx------   2 root root 4096 Jun 10 00:00 root
drwxr-xr-x   3 root root 4096 Jun 10 00:00 run
drwxr-xr-x   2 root root 4096 Jun 10 00:00 sbin
drwxr-xr-x   2 root root 4096 Jun 10 00:00 srv
dr-xr-xr-x  13 root root    0 Jun 18 09:30 sys
drwxrwxrwt   1 root root 4096 Jun 11 00:03 tmp
drwxr-xr-x   1 root root 4096 Jun 10 00:00 usr
drwxr-xr-x   1 root root 4096 Jun 10 00:00 var
```
- 详细目录
    1. nginx.conf配置文件路径 ==/etc/nginx/nginx.conf==
    2. default.conf配置文件的路径 ==/etc/nginx/conf.d/default.conf==
    3. 默认首页文件夹html路径 ==/usr/share/nginx/html==
    4. 日志文件路径 ==/var/log/nginx==

### 5. 挂载文件夹
```
# 普通的挂载方式

// 创建本地 nginx 目录，存放相关配置
mkdir -p ~/nginx/www ~/nginx/logs ~/nginx/conf

// 拷贝docker nginx 配置文件到本地
docker cp cafa:/etc/nginx/nginx.conf ~/nginx/conf

docker container  run \
-d \
-p 8090:80 \
--name mynginx \
-v ~/nginx/www:/usr/share/nginx/html \
-v ~/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v ~/nginx/logs:/var/log/nginx \
nginx

```
> #### 命令参数解析
- -d：在后台运行
- -p ：容器的80端口映射到 8090:80
- --rm：容器停止运行后，自动删除容器文件
- --name：容器的名字为mynginx

## 自签名证书
为容器加入 HTTPS 支持，第一件事就是生成私钥和证书。正式的证书需要证书当局（CA）的签名

> 未完待续

## HTTPS 配置

> 未完待续

## docker 常用命令
### 查看命令
#### 1. 查看镜像命令
```
docker images
```
#### 2. 查看容器命令
```
# 使用 docker ps -a 命令即可列出所有（包括已停止的）的容器
docker ps -a
```
### 启动命令
#### 容器启动命令
```
docker start [NAME]/[CONTAINER ID]
docker restart container-name
```
### 终止命令
#### 容器终止命令
```
docker container stop [NAME]/[CONTAINER ID]:将容器退出。
docker container kill [NAME]/[CONTAINER ID]:强制停止一个容器。
```
### 删除命令
####
1. 删除一个容器--==不能够删除一个正在运行的容器，会报错。需要先停止容器。==
```
docker rm [NAME]/[CONTAINER ID]:
```
2.删除所有容器
```
-a标志列出所有容器
-q标志只列出容器的ID，然后传递给rm命令，依次删除容器
docker rm $(docker ps -aq)

```
## 参考文档
【1】[docker从入门到实践](https://yeasy.gitbooks.io/docker_practice/introduction/)
【2】[docker部署nginx并且挂载文件夹和文件](https://blog.csdn.net/qq_26614295/article/details/80505246)
【3】[Nginx 容器教程](http://www.ruanyifeng.com/blog/2018/02/nginx-docker.html)