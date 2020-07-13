---
title: 基于fork的git常规操作
date: 2019-06-11 20:02:21
tags: [git, fork]
categories: git
---

![image](https://i.loli.net/2019/06/11/5cff98b6b918b27168.png)

## 一、fork是什么

我们在多人协同开发项目的过程中，git是必不可少的代码托管工具。但是繁琐的操作命令、抽象的文件状态就需要花费我们大量的时间。

> **fork**拥有可视化界面的项目版本控制软件，适用于git项目管理。

现在很多人都在用sourcetree，至于你问我为什么不用sourcetree，这完全是个人喜好，在我看来**fork**相比sourcetree拥有更简约的风格，还有其**更直观而且方便操作的文件树形视图**。

## 二、常用操作命令

![image](https://i.loli.net/2019/06/11/5cff98d9ad88f64240.png)


### fetch

`git fetch`命令通常用于将远程仓库同步到本地仓库,但是不会对你工作空间产生影响。

![image](https://i.loli.net/2019/06/11/5cff98f50df5022266.png)

### pull

`git pull`命令用于将远程仓库同步到本地仓库,并且合入到工作空间。

> git pull = git fetch + git merge

![image](https://i.loli.net/2019/06/11/5cff99130667236449.png)

这里不建议使用rebase代替merge，rebase通常用于分支合并的时候。

### commit

`git commit`命令是将工作空间修改的内容提交至本地仓库。

![image](https://i.loli.net/2019/06/11/5cff992d9225b19032.png)

修改文件和暂存文件之间可通过`双击`自由切换，当然也可以文件的部分提交，最后commit至本地仓库。

### push

`git push`命令用于将本地分支的更新，推送到远程仓库。

![image](https://i.loli.net/2019/06/11/5cff994ab75ea64406.png)

这里有6次commit的修改需要push。

### revert commit

`git revert commit`命令是撤销某一次commit，即删除该次提交的修改。

![image](https://i.loli.net/2019/06/11/5cff996272eac13307.png)

如图，当我们撤销`蒙层不可点击`commit时：

![image](https://i.loli.net/2019/06/11/5cff997b3f1dd74783.png)

修改已经删除。

### reset

`git reset`命令是回滚到之前的某个状态。

![image](https://i.loli.net/2019/06/11/5cff99957acac11068.png)

![image](https://i.loli.net/2019/06/11/5cff99a888f7a83639.png)

有三种回滚方式，这里就不做介绍了。

> 如果远程仓库已在本地仓库之上，请使用**git push xxx --force**强制提交。

### stash

`git stash`命令是将本地工作空间**所有修改**暂存到stash，并且随时可以取出。

常用的应用场景就是`解决冲突`和`切换分支`.

详细使用方法请参考[git切换分支保存修改的代码的方法](http://blog.yangyong.io/2019/05/15/git/git%E5%88%87%E6%8D%A2%E5%88%86%E6%94%AF%E4%BF%9D%E5%AD%98%E4%BF%AE%E6%94%B9%E7%9A%84%E4%BB%A3%E7%A0%81%E7%9A%84%E6%96%B9%E6%B3%95/)

### cherry-pick

`git cherry-pick`是将**某一次提交的修改**，合入到本地仓库以及工作空间。

![image](https://i.loli.net/2019/06/11/5cff99cc4540d89941.png)

如图，我想合入`顶部增加一空行`到本地，而没有合入`蒙层不可点击`的代码。

这就是**cherry-pick**与分支**merge**的区别，merge是将所有提交合入。

### rebase

`git rebase`命令是变基操作，即将一个分支合入到另一个分支。

那么它与merge有什么不同呢。

#### merge

- 创建一个新的merge commit
- 优点：记录了真实的commit情况，包括每个分支的详情
- 缺点：因为每次merge会自动产生一个merge commit，所以在使用一些git 的GUI tools，特别是commit比较频繁时，看到分支很杂乱。

![image](https://i.loli.net/2019/06/11/5cff99e9498ab80047.png)

#### rebase

- **会合并之前的commit历史**
- 得到更简洁的项目历史，去掉了merge commit
- 如果合并出现代码问题不容易定位，因为re-write了history

![image](https://i.loli.net/2019/06/11/5cff9a0148fa521253.png)

> 注意！！！变基会有重写提交历史的风险。

![image](https://i.loli.net/2019/06/11/5cff9a168c22721609.png)

如图，如果你rebase master 到你的feature分支：
rebase 将所有master的commit移动到你的feature 的顶端。

问题是：其他人还在 master上开发，由于你使用了rebase移动了master，git 会认为你的主分支的历史与其他人的有分歧，会产生冲突。

> 所以，rebase不应使用在并行开发的分支。

如果你想要一个干净的，没有merge commit的线性历史树，那么你应该选择**git rebase**
如果你想保留完整的历史记录，并且想要避免重写commit history的风险，你应该选择使用**git merge**

## 三、总结

作为程序员，git是必不可少的管理工具，如果熟练运用，能够很大的提升我们开发规范与开发效率。

灵感来源：通过外包项目，遇到协作的问题，写下此篇博客，更感谢爽能的大力支持。