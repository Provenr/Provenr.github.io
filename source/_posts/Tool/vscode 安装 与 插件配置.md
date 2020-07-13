---
title: vscode 安装 与 插件配置
date: 2018-10-20 00:34:19
tags: [vscode, git, sourceTree]
description: 
---



# vscode 下载安装

1. 下载安装 官网下载
[下载链接](https://code.visualstudio.com/)
2. 设置 语言模式

    2.1 快捷键
    ```
    Windows、Linux 快捷键是：ctrl+shift+p
    
    macOS 快捷键是：command + shift + p
    ```
    2.2 搜索：***配置语言*** 或者  ***Configure** Language*
    
    2.2 选择后会打开 locale.json 文件
    
    > 当你删除之前的语言设置，在“locale”后面输入冒号或引号时会提示可用的选项
    
    ```
    "locale": "zh-cn"
    ```
    
    > 保存对 locale.json 文件的修改，关闭VSCODE，重新打开语言就变了。
    
    > 
    
    > ==**Tips**==: 重启之后语言还不行
    
    > 安装插件  
    
    ```
    Chinese (Simplified) Language Pack for Visual Studio Code ms-ceintl.vscode-language-pack-zh-hans
    ```
# 前端开发相关插件

- HTML Snippets

    超级实用且初级的 H5代码片段以及提示
    
- HTML CSS Support

    让 html 标签上写class 智能提示当前项目所支持的样式
    
- Path Intellisense

    自动路劲补全
- Npm Intellisense

    require 时的包提示（最新版的vscode已经集成此功能）
- Document this
  
  js 的注释模板 （注意：新版的vscode已经原生支持,在function上输入/** tab）

- beautify

    格式化代码的工具
    
- Atuo Rename Tag

    修改 html 标签，自动帮你完成尾部闭合标签的同步修改
    
- fileheader

    顶部注释模板，可定义作者、时间等信息，并会自动更新最后修改时间

- filesize

    在底部状态栏显示当前文件大小，点击后还可以看到详细创建、修改时间
    
- Bracket Pair Colorizer

    让括号拥有独立的颜色，易于区分。可以配合任意主题使用
    
高级插件 [查看](https://zhuanlan.zhihu.com/p/27905838)


先安装 homebrew  来安装appStore不存在的，但是需要的

```
    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Homebrew的使用
- 安装软件：brew install 软件名，例：brew install wget
- 搜索软件：brew search 软件名，例：brew search wget
- 卸载软件：brew uninstall 软件名，例：brew uninstall wget
- 更新所有软件：brew update

    ⚠️通过 update 可以把包信息更新到最新，不过包更新是通过git命令，所以要先通过 brew install git 命令安装git。

- 更新具体软件：brew upgrade 软件名 ，例：brew upgrade git
- 显示已安装软件：brew list
- 查看软件信息：brew info／home  软件名，

    例：brew info git ／ brew home git
⚠️brew home指令是用浏览器打开官方网页查看软件信息
- 查看那些已安装的程序需要更新： brew outdated
- 显示包依赖：brew reps

>###  ==Tips:== 一定要 **开启VPN** 

Homebrew安装成功后，会自动创建目录 /usr/local/Cellar 来存放Homebrew安装的程序。 这时你在命令行状态下面就可以使用 brew 命令了

## 通过brew 安装git

```
    brew install git

```

1. command + space 唤起聚焦搜素
2. 搜索终端 进入命令行
3. git 配置

> 设置Git的user name和email：
```
    git config user.name "xxxxx"
    git config user.email "xxxxxxx@qq.com"
```
> 二、生成SSH密钥过程：

1. 查看是否已经有了ssh密钥：cd ~/.ssh
如果没有密钥则不会有此文件夹，有则备份删除

2. 生存密钥：
$ ssh-keygen -t rsa -C “xxxx@qq.com”

**==Tips:==**  连续按3个回车，密码为空。

4. 最后得到了两个文件：id_rsa和id_rsa.pub

5. 在github上添加ssh密钥，这要添加的是“id_rsa.pub”里面的公钥。

> 打开终端 查找id_rsa.pub

```
    open ~/.ssh  
```

[sourceTree 安装使用](https://www.jianshu.com/p/fdbf7c0bca93)

==Tips:== 需要打开VPN 否则无法登陆