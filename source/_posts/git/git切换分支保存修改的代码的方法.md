---
title: git切换分支保存修改的代码的方法
date: 2019-05-15 20:29:26
tags: [git, git stash]
categories: git
---

![image](https://i.loli.net/2019/05/15/5cdbfb32ba9fc79837.png)

最近在一个原有的项目上做一次非常大的改版，底层的数据库做了很大的变化，跟现在的版本无法兼容。现在的工作除了开发最新的版本之外还要对原来的版本做例行的维护，修修补补。于是有了在两个分支之间游走切换的问题，最新改版的代码在分支new上，旧版本的代码在分支old上，我在new上开发了一半，忽然有人给了我一个改进的需求，于是我要切换回old去修改代码。在这个场景下，我摸索了四种方法：

## 1. 及时commit代码

在new分支上把已经开发完成的部分代码commit掉，不push，然后切换到old分支修改代码，做完了commit，所有分支互不影响，这是一个理想的方法。

## 2. 使用git stash

有时候写了一半的JAVA代码，都还不能编译通过的，就被叫去改另一个分支的bug了。

在new分支上的时候在命令行输入：

```
git stash
```

或者

```
git stash save "修改的信息"
```

这样以后你的代码就回到自己上一个`commit`了，直接`git stash`的话git stash的栈会直接给你一个`hash值`作为版本的说明，如果用`git stash save “修改的信息”`，git stash的栈会把你填写的“修改的信息”作为版本的说明。

接下来你回到`old分支`修改代码完成，你又再回到`new分支`，输入：

```
git stash pop
```

或者

```
git stash list
```

```
git stash list
```

就可以回到保存的版本了。`git stash pop`的作用是将git stash栈中最后一个版本取出来，`git stash apply stash@{0}`的作用是可以指定栈中的一个版本，通过`git stash list`可以看到所有的版本信息：

```
stash@{0}: On develop/2019515: yangyong's stashing
```

```
stash@{1}: On develop/2019515: yangyong's stashing2
```

然后你可以选择一个你需要的版本执行：

```
git stash apply stash@{0}
```

这时候你搁置的代码就回来了。

## 3. Fork(I use most commonly)

- Save Stash

![image](https://i.loli.net/2019/05/15/5cdc06e62604a58644.png)

- Save by a instructions

![image](https://i.loli.net/2019/05/15/5cdc0719c3ce985204.png)

- Saved the changes

modified files already saved in this stash`(yangyong's stash)`. you can take back them anytime.
![image](https://i.loli.net/2019/05/15/5cdc098e9a0ad28310.png)
- **Then change branch or do something that you need to do**
- Apply the stash

![image](https://i.loli.net/2019/05/15/5cdc0762cdde894049.png)

- Applied the stash (changes back)

![image](https://i.loli.net/2019/05/15/5cdc07929a4cc25349.png)

- Delete the stash

![image](https://i.loli.net/2019/05/15/5cdc07b76da9c48692.png)

## 4. 用IDE工具的shelve的功能

有一些IDE工具提供了shelve的功能，shelve的意思是“将…搁在一边”，即把还没写完的代码先搁在一边。我开发都是使用jetbrains公司的IDEA和PhpStorm，它们就提供了shelve的功能，方法：

首先在IDE的底部找到“Changes”，点开会有local的选项卡，选中你要搁置的代码，点击右键，选择“Shelve Changes”，在提交的输入框中输入你的注释，以便回来的时候识别你需要的版本，点击“Shelve Changes”键即可。这时选项卡上会多一个“Shelf”的选项卡，里面就有你搁置的代码。

这时候你可以去old分支修改代码，改完了之后回到new分支，到“Shelf”选项卡下选择你要恢复的代码或者版本，点击右键选择“Unshelve Changes”，你的搁置的代码就回来了。

## 参考

1. [Git 工具 - 储藏（Stashing）](https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E5%82%A8%E8%97%8F%EF%BC%88Stashing%EF%BC%89)
2. [git切换分支保存修改的代码的方法](http://www.tonitech.com/2344.html)
