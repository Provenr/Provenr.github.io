---
title: iterm + zsh 打造Mac个性终端
tags:
  - iterm2
  - 终端
date: 2020-03-23 20:38:41
---
## iterm2 安装
iTerm2官方下载地址 [http://www.iterm2.com/downloads.html](https://www.iterm2.com/downloads.html)

## zsh bash

1. 通过 ==cat /etc/shells== 命令可以查看当前系统可以使用哪些shell；
2. 通过 ==echo $SHELL== 命令可以查看我们当前正在使用的shell；
3. 通过==chsh -s /bin/zsh==命令可以将shell切换为shell之zsh，终端重启之后即可生效。
4. 4.将shell切换为zsh之后，我们就可以安装Oh My ZSH了
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## 配置主题
安装成功之后，我们可以通过vi ~/.zshrc，设置ZSH_THEME="agnoster"对主题进行修改。
主题在线预览[https://github.com/robbyrussell/oh-my-zsh/wiki/Themes](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)

## 注意，agnoster主题能否设置成功，还依赖于以下东西：

1. [Solarized Dark配色方案](http://ethanschoonover.com/solarized)

下载完成之后解压，在iTerm2的Preferences——Profiles——colors——Load Presets——Solarized Dark即可设置终端配色
![image](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/E46041C9B1D646C9A24B10173C9868D8/5293)

2. [特殊字体安装](https://github.com/powerline/fonts)
将以下命令在你的终端中执行：
```
# clone
git clone https://github.com/powerline/fonts.git --depth=1
# install
cd fonts
./install.sh
# clean-up a bit
cd ..
rm -rf fonts

```

下载完成之后解压，执行其中的install.sh文件，在iTerm2的Preferences——Profiles——Text中同时将Regular Font和Non—ASCII Font设置为Meslo LG M DZ Regular for Powerline即可
![image](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/DB05875C2B49454E9EB3AB93DBF4794E/5295)

3. 也支持背景图替换
打开路径:iterm2 -> Preferences -> Profiles -> window -> Background Image
选择一张自己喜欢的壁纸即可
可以通过Blending调节壁纸的透明度: 透明度为0的时候,背景变为纯色(黑色)

4. 安装语法高亮插件
这是oh my zsh的一个插件，安装方式与theme大同小异：
```
cd ~/.oh-my-zsh/custom/plugins/
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
```
打开zshrc文件进行编辑
```
vi ~/.zshrc
```
找到plugins，此时plugins中应该已经有了git，我们需要把高亮插件也加上,请务必保证插件顺序，zsh-syntax-highlighting必须在最后一个
然后在文件的最后一行添加
```
source ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```
按一下esc调出vi命令，输入:wq保存并退出vi模式

执行命令使刚才的修改生效：
```
source ~/.zshrc
```

## 最终效果
![image](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/3566337D36E749DCB38C6100460D0311/5321)
## 番外篇
利用cdto 小工具 在Finder中自定义工具栏，在终端中打开当前目录。
[cdto下载链接](https://github.com/jbtule/cdto)

1. 解压缩文件，找到当前符合的cdto app，使用了iterm选择iterm下的cdto.app，默认终端选择terminal下的cdto.app
![image](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/E37BF972597C4F2B8A84470A8DFD8C6D/5319)
2. 将cdto.app 拖拽到Application中
3. 在Application中 找到cdto.app, 按住 Command 键的同时将项目拖到工具栏中直至看到绿色加号。
![image](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/67A4506410A2441E8D2960D78D0D2C9C/5337)
4. 利用 Alfred 启动 配置  terminal 启动iterm 
```
on alfred_script(q)
 tell application "System Events"
  get name of every process whose name is "iTerm"
  tell application "iTerm"
   set newWindow to (create window with default profile)
   tell current session of newWindow
    write text q
   end tell
  end tell
 end tell
end alfred_script

```