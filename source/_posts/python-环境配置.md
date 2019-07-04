---
title: python 环境配置
date: 2019-07-04 23:38:24
tags: [python, Anaconda]
---
Windows 环境安装

一、Anaconda 的下载及安装
1. Anaconda 是什么
Anaconda 是 Python 的包管理器和环境管理器。
2.安装了 Anaconda 还需要安装 Python 吗?
不需要，原因有以下几点:
1) Anaconda 提供了一个编译好的环境可以直接安装。
2) Anaconda 附带了一大批常用数据科学包，它附带了 conda、Python 和 150 多个科学包 及其依赖项。因此你可以用 Anaconda 立即开始处理数据。
3) Anaconda 是 Python 的一个科学计算发行版，内置了数百个 Python 经常会使用的库，也 包括做机器学习或数据挖掘的库，如 Scikit-learn、NumPy、SciPy 和 Pandas 等，其中可能有 一些是 TensorFlow 的依赖库。
3. 如何下载安装?
Anaconda3-4.4.0-Windows-x86_64下载链接:
https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-4.4.0-Windows-
x86_64.exe
(如需下载其他操作系统对应版本，请复制下方链接到浏览器中打开: https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)
选择对应版本的安装包进行下载，下载完成后直接安装。 Ps:确认勾选将 Python 添加到系统环境变量。

二、集成开发环境 IDE: PyCharm 的下载及安装
PyCharm 是由 JetBrains 打造的一款 Python IDE，支持 macOS、 Windows、 Linux 系统。 PyCharm 功能 : 调试、语法高亮、Project 管理、代码跳转、智能提示、自动完成、单元测 试、版本控制......
PyCharm 下载地址:https://www.jetbrains.com/pycharm/download/
1) 进入该网站后，我们会看到如下界面:

professional 表示专业版，community 是社区版，推荐安装社区版，因为是免费使用的。
2) 双击下载的安装包，进行安装，然后会弹出界面:

3) 选择安装目录，Pycharm 需要的内存较多，建议将其安装在 D 盘或者 E 盘，不建议放在 系统盘 C 盘:

4)点击 Next，进入下图的界面:

Create Desktop Shortcut 创建桌面快捷方式，一个 32 位，一个 64 位，小编的电脑是 64 位系 统，所以选择 64 位。
勾选 Create Associations 是否关联文件，选择以后打开.py 文件就会用 PyCharm 打开。
5)点击 Next，进入下图:

默认安装即可，直接点击 Install。 6)耐心等待安装完成。


Mac 环境安装
一、 Python安装
1.Python 安装与否及版本检测
1）Command + space 打开搜索

 2） 在在Finder中Applications/Utilities(或桌面应用程序，选择Terminal，打开一个终 端端口

2)执行命令 python(注意，其中p是小写的)，输出结果即为安装的
Python 版本。最后的>>>是一个提示符，表示在其之后可以输入Python的命令。

以上输出结果显示 Python 已安装，版本为 Python 2.7.10。
检查系统是否安装了 Python 3，可尝试执行命令‘python3’。如果返回一条 错误的信息，则系统未安装 Python 3。如若返回指出系统安装了 Python3，则无 需安装就可使用它。
2、安装Python 3.6
1)从 Python 官方网站下载 Python 3.6 安装包。
下载地址:https://www.python.org/downloads/鼠标点击“Download Python 3.6.4”按钮。

随之进入下载阶段，点击右上角“显示下载”按钮可查看下载剩余时间。
2) 下载完成后双击安装包，在不断弹出的对话框中点击继续。

3) 选择同意软件许可协议中的条款。

4) 点击安装开始安装程序。

5) 开始安装进度显示。

6) 安装成功，点击关闭。

7) 安装完成，检测 Python 版本。
打开终端，执行命令“python”，输出结果若显示为 Python 3 版本，则表 明安装成功。
二、Pycharm开发工具安装
接下来，您需要安装一个 Python 文本编辑器来创建和编辑 Python 文件。在本次课程 中，我们将会使用 Pycharm 文本编辑器。Pycharm 的配置方法如下:
1)下载 Pycharm 安装包
下载地址:Pycharm 官方网站(http://www.jetbrains.com/pycharm/) 鼠标点击“DOWNLOAD NOW”。
professional 表示专业版，community 是社区版，推荐安装社区版，因为是免费使用的。
2)下载完成之后，双击安装包，然后将 Pycharm CE 图标拖入 Application 文件夹中 即可。

3)双击应用程序中的 Pycharm CE 图标，点击 Create New Project，选择保存路径， 点击 Create 即可，一个新的 Project 已经创建好编辑器。

4)点击工具栏中的 File->New->Python File。到此，Python 编辑文件就已经创建好 了，Python 之旅即可从此开始。

三、Anaconda 的下载及安装
Anaconda3-4.4.0-MacOSX-x86_64 下载链接:
https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/
Anaconda3-4.4.0-MacOSX-%20x86_64.pkg
(如需下载其他操作系统对应版本，请复制下方链接到浏览器中打开: https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)
选择对应版本的安装包进行下载，下载完成后直接安装。 Ps:确认勾选将 Python 添加到系统环境变量。