---
title: git commit 工作流的标准姿势以及 release note 自动生成
date: 2018-09-24 13:55:20
tags: git
categories: git
---

一个项目除了前期的开发任务以外，还有后期的功能迭代维护，面对较为频繁的功能上线和一些繁杂的事情，于是有条不紊地管理我们的 release notes 是一门学问。

之前项目功能迭代上线，版本通过时间戳，更新内容更是通过人力去维护，时间跨度一大这种人力维护 release notes 变得混乱。

于是考虑通过规范的 commit message 来自动生成 release notes。

### 规范标准(commit)
主流的 commit message 规范为：
```
<type>(<scope>): <subject>

<body>

<footer>
```
1. **标准类别(type):**
用于声明此次commit的主要目的类别：
```
feat：feature、发布新功能
fix：修复bug 
docs：更新文档
style： 代码格式 
refactor：代码重构 
test：增加测试 
chore：构建过程或辅助工具的变动
```

2. **范围(scope):**
用于说明commit影响的范围；如数据层(model)，视图层(view)，控制层（controller）等。

3. **描述(subject):**
commit的主题描述，少于50个字。

4. **body/footer:**
用于详细描述和关闭issue的补充。(skipped)

------
### 使用commitizen代替commit
[Commitizen](https://github.com/commitizen/cz-cli)是一个撰写合格 Commit message 的工具。

安装命令如下:
```
npm install -g commitizen
```
然后，在项目目录里，运行下面的命令，使其支持 Angular 的 Commit message 格式:
```
commitizen init cz-conventional-changelog --save --save-exact
```
以后，凡是用到git commit命令，一律改为使用git cz。这时，就会出现选项，用来生成符合格式的 Commit message。

![gitcz](https://github.com/AngusYang9/image/blob/master/blog/git%20commit%20%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%9A%84%E6%A0%87%E5%87%86%E5%A7%BF%E5%8A%BF%E4%BB%A5%E5%8F%8A%20release%20note%20%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90/githubImage%20%5B~:Documents:angus:githubImage%5D%20-%20...:package.json%20%5BgithubImage%5D%202019-01-29%2014-37-08.png?raw=true)

-----
### 规范化commit-msg / commitlint
这里我们使用另一个git hooks：commitmsg，我们来安装validate-commit-msg检查 Commit message 是否符合格式。
```
yarn add husky // git hooks
yarn add validate-commit-msg
```
在package.json中配置：
```json
"config": {
    "commitizen": {
      "path": "./node_modules/cz-conventional-changelog"
    }
  }
```
如果要进行自定义配置，我们可以自建一个文件.vcmrc:
```json
{
  "types": ["feat", "fix", "docs", "style", "refactor", "perf", "test", "build", "ci", "chore", "revert",":art"],
  "scope": {
    "required": false,
    "allowed": ["*"],
    "validate": false,
    "multiple": false
  },
  "warnOnFail": false,
  "maxSubjectLength": 100,
  "helpMessage": "",
  "autoFix": false
}
```
> #### 关于git hooks
提到“hooks”这个词我们应该并不陌生，比如vue和react都有自己的lifecycle hooks，在git中分为客户端hooks和服务端hooks。在commit阶段中涉及到的是客户端hooks，其中客户端hooks包含：

>  `pre-commit`钩子在键入提交信息前运行。 它用于检查即将提交的快照，例如，检查是否有所遗漏，确保测试运行，以及核查代码。 如果该钩子以非零值退出，Git 将放弃此次提交，不过你可以用 git commit --no-verify 来绕过这个环节。 你可以利用该钩子，来检查代码风格是否一致（运行类似 lint 的程序）、尾随空白字符是否存在（自带的钩子就是这么做的），或新方法的文档是否适当。

> `prepare-commit-msg` 钩子在启动提交信息编辑器之前，默认信息被创建之后运行。 它允许你编辑提交者所看到的默认信息。 该钩子接收一些选项：存有当前提交信息的文件的路径、提交类型和修补提交的提交的 SHA-1 校验。 它对一般的提交来说并没有什么用；然而对那些会自动产生默认信息的提交，如提交信息模板、合并提交、压缩提交和修订提交等非常实用。 你可以结合提交模板来使用它，动态地插入信息。

> `commit-msg` 钩子接收一个参数，此参数即上文提到的，存有当前提交信息的临时文件的路径。 如果该钩子脚本以非零值退出，Git 将放弃提交，因此，可以用来在提交通过前验证项目状态或提交信息。

> ` post-commit` 钩子在整个提交过程完成后运行。 它不接收任何参数，但你可以很容易地通过运行 git log -1 HEAD 来获得最后一次的提交信息。 该钩子一般用于通知之类的事情。 

-----
### 生成 Change log
如果你的所有 Commit 都符合 Angular 格式，那么发布新版本时， Change log 就可以用脚本自动生成。

生成的文档包括以下三个部分:
> - New features
- Bug fixes
- Breaking changes

每个部分都会罗列相关的 commit ，并且有指向这些 commit 的链接。当然，生成的文档允许手动修改，所以发布前，你还可以添加其他内容。

[conventional-changelog](https://github.com/ajoslin/conventional-changelog) 就是生成 Change log 的工具，运行下面的命令即可:
```
npm install -g conventional-changelog
```

上面命令不会覆盖以前的 Change log，只会在CHANGELOG.md的头部加上自从上次发布以来的变动。

如果你想生成所有发布的 Change log，要改为运行下面的命令:
```
conventional-changelog -p angular -i CHANGELOG.md -w -r 0
```

为了方便使用，可以将其写入package.json的scripts字段

```json
{
  "scripts": {
    ... ...
    // 按类别排序 只包含Angular格式
    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -w -s -r 0 && git add CHANGELOG.md"
    // 按提交时间排序
    "changelog": "conventional-changelog -p -i CHANGELOG.md -w -s -r 0 && git add CHANGELOG.md"
  }
}

```
以后，直接运行下面的命令即可
```
npm run changelog
```

有时出现command not found，以为是conventional-changelog没有安装，通过命令：
```
npm ls -g -depth=0
```
打印出：
```
/usr/local/lib
├── commitizen@2.9.6
├── conventional-changelog@1.1.0
├── cz-conventional-changelog@2.0.0
└── npm@4.3.0
```
明明是有的，苦思不得其解，最后在这篇文章[Git 提交记录和分支模型](https://link.jianshu.com?t=https://cattail.me/tech/2016/06/06/git-commit-message-and-branching-model.html)中发现Commitizen就依据conventional message，创建起一个生态：
- `conventional-changelog-cli`: 通过提交记录生成 CHANGELOG.md
- `conventional-github-releaser`: 通过提交记录生成 github release 中的变更描述
- `conventional-recommended-bump`: 根据提交记录判断需要升级 Semantic Versioning 哪一位版本号
- `validate-commit-msg`: 检查提交记录是否符合约定

于是就改用了conventional-changelog-cli：
```
npm install -g conventional-changelog-cli
cd #project
conventional-changelog -p angular -i CHANGELOG.md -s
```
通过以上命令你就会发现在项目中多了个CHANGELOG.md文件，表示生成 Change log成功了。

---
** `changelog 支持中英文 `**

在 package.json 中配置
```json
{
  "lang": "zh" // 'zh' | 'cn'
}
```
---
样例:
![gitcz](https://angusyang9.github.io/css/images/git/changelog.png)

### 相关
- [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog) : 根据 commit message 生成 changelog
- [commitlint](https://github.com/marionebl/commitlint) : Lint commit messages
- [@baidu/conventional-changelog-befe](http://gitlab.baidu.com/be-fe/conventional-changelog-befe) : conventional-changelog BEFE 预设
- [@baidu/commitlint-config-befe](http://gitlab.baidu.com/be-fe/commitlint-config-befe) : commitlint lint BEFE 预设
