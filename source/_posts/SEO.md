---
title: SEO
date: 2019-06-16 17:28:40
tags: seo
---
### SEO 工作的目的
是为了让网站更利于让各大搜索引擎抓取和收录，增加产品的曝光率
### SEO 注意事项
#### 1. 网站 TDK 标签的设置
title,description,keywords

```
<title>淘宝网 - 淘！我喜欢</title>
<meta name="description" content="淘宝网 - 亚洲较大的网上交易平台，提供各类服饰、美容、家居、数码、话费/点卡充值… 数亿优质商品，同时提供担保交易(先收货后付款)等安全交易保障服务，并由商家提供退货承诺、破损补寄等消费者保障服务，让你安心享受网上购物乐趣！">
<meta name="keyword" content="淘宝,掏宝,网上购物,C2C,在线交易,交易市场,网上交易,交易市场,网上买,网上卖,购物网站,团购,网上贸易,安全购物,电子商务,放心买,供应,买卖信息,网店,一口价,拍卖,网上开店,网络购物,打折,免费开店,网购,频道,店铺">
```

还有 meta 的 canonical 设置，一个网站还通过多个 url 访问，canonical 就是用来 告诉搜索引起，这么多个 url 中最有价值最重要的一个 url，一般是网站的首页
```
<link ref="canonical" href="">
```

#### 2. 建立 robots.txt 文件
robots 文件是搜索引擎登录网站第一个访问的文件，robots 可以设置允许被访问的搜索引擎，最主要的还是设置
==允许 Allow== 和
==不允许 Disallow ==访问 的目录和文件，少写 Disallow，多写 Allow，用意是引导爬虫抓取网站的信息。
另外， 在 ==robots 文件底部指明网站 sitemap 文件的目录==，爬虫读取其中的 sitemap 路 径，接着抓取其中相链接的网页。提高网站的收录量。

#### **==案例:==**

[https://www.taobao.com/robots](https://www.taobao.com/robots.txt)

#### 3. 建立网站的 sitemap 地图文件


sitemap 是一个将网站栏目和连接归类的一个文件， 可以更好地将网站展示给搜索引擎，提高爬虫的爬取效率。sitemap 地图文件==包含 html(针对用户)和 xml(针对搜索引擎)两种==。
当网站更新频繁的时候，sitemap 文件要做到自动更新(程序实现)，更新不频繁的可以手 动更新提交。
> 1. 将sitemap.xml和robort.txt放在网站的根目录下
>
> 2. Sitemap.html格式的网站地图主要用来方便用户的浏览使用，并不能起到 XML Sitemap 所起的作用。所以最好是两者都要有。

#### **==案例:==**
[http://www.sitemap-xml.org/](http://www.sitemap-xml.org/)
```

<?xml version="1.0" encoding="UTF-8"?>
<urlset
    xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9
       http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd">
<url>
<loc>http://m.ponhu.cn</loc>
<priority>1.00</priority>
<lastmod>2019-03-19</lastmod>
<changefreq>weekly</changefreq>
</url>
</urlset>

```

#### 4. 图片 img 标签必须加上 alt 属性

```
<img class="s_lg_img_gold_show" src="https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo_top_86d58ae1.png" alt="到百度首页" title="到百度首页">
```

#### 5. h1~h6 标签合理使用。
按道理所有标签都需要根据自身的语气合理去使用，这里不展 开讲述，这里只讲 h 标签的注意事项。==h1 标签一个页面只能出现 1 次==，h2 标签一般作为 二级标题或者文章的小标题。==最合理的使用时 h1~h6 按顺序层层嵌套下去，不可以断层==

#### 6. 设置 nofollow 属性值

a 标签的 rel="nofollow"， 表示不希望搜索引擎继续追踪的 链接，取消这个链接在整站所占的权重比。一般是其他网站的链接、a 标签启动 QQ 聊 天，webapp 拨打电话。

#### 7. 安装百度的自动推送代码

当页面被访问的时候，页面的 url 会自动推送给百度 搜索引擎，有利于网页更快地被百度发现。

#### **==案例:==**
```
<script>
    (function(){
       var bp = document.createElement('script');
       var curProtocol = window.location.protocol.split(':')[0];
       if (curProtocol === 'https'){
           bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';
       }
       else{
           bp.src = 'http://push.zhanzhang.baidu.com/push.js';
       }
       var s = document.getElementsByTagName("script")[0];
       s.parentNode.insertBefore(bp, s);
    })();
</script>
```

#### 8. 增加网站的 404页面

一个是利于用户体验，最主要的是==防止蜘蛛爬虫的丢失。== 但有一点要注意，==不要设置自动跳转到首页，会被搜索引擎认为是在作弊==，你在 404 页面 设置一个引导链接让用户自己点就可以。