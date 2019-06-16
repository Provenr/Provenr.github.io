# HomePage
个人博客

# Provenr-blog
Hexo+TravisCI+theme-replica build provenr blog

git clone https://github.com/sabrinaluo/hexo-theme-replica

npm install hexo-renderer-jade@0.3.0 --save
npm install hexo-renderer-stylus --save

## Commander
  - `hexo new "postName"` #新建文章
  - `hexo new page "pageName"` #新建页面
  - `hexo generate` #生成静态页面至public目录
  - `hexo server` #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
  - `hexo deploy` #部署到GitHub
  - `hexo help`  # 查看帮助
  - `hexo version`  #查看Hexo的版本

 ----
 **Abbreviation**
 - `hexo n == hexo new`
 - `hexo g == hexo generate`
 - `hexo s == hexo server`
 - `hexo d == hexo deploy`

 **combination**
 - `hexo s -g` #生成并本地预览
 - `hexo d -g` #生成并上传

 markdown title style
 ---
 - `title: postName` #文章页面上的显示名称，一般是中文
 - `date: 2013-12-02 15:30:16` #文章生成时间，一般不改，当然也可以任意修改
 - `categories: 默认分类` #分类
 - `tags: [tag1,tag2,tag3]` #文章标签，可空，多标签请用格式，注意:后面有个空格
 - `description: 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面`
 ---

###在项目目录里，运行下面的命令，使其支持 Angular 的 Commit message 格式。
commitizen init cz-conventional-changelog --save --save-exact

- feat：新功能（feature）
- fix：修补bug
- docs：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动
