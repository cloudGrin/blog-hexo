---
title: 使用 Hexo + GitHub + Netlify + Cloudflare 搭建个人博客
categories: Hexo
abbrlink: '8585'
date: 2020-04-12 23:08:58
tags:
---

![bird](https://cloudgrin.oss-cn-shanghai.aliyuncs.com/images/WechatIMG20.jpeg?x-oss-process=image/resize,w_600,limit_0)

## 前言

对于程序员，写博客的重要性不言而喻，借用网上的一张图（侵删）：

![](https://user-gold-cdn.xitu.io/2020/4/12/1716d05ce1a3118b?w=1333&h=605&f=png&s=447856)

而且只要一杯瑞幸的钱😄（买域名，不买的话完全是白嫖啊）就可以拥有自己的一篇空间，想想就很兴奋，是不是。

以下除了具体流程，也会列出我在实践的过程中踩了一些坑，希望能帮助到大家。

### 为什么选择 Hexo + GitHub + Netlify + Cloudflare
不需要云服务器，不需要备案，全部免费
#### Hexo

[Hexo](https://hexo.io/zh-cn/) 是一个基于 nodejs 的静态博客网站生成器，作者是来自台湾的 Tommy Chen。

* 静态文件快速生成
* 支持 Markdown
* 仅需一道指令即可部署到 GitHub Pages 和 Heroku
* 已移植 Octopress 插件
* 高扩展性、自订性
* 兼容于 Windows, Mac & Linux
* 社区主题、插件很多，适合喜欢折腾的小伙伴

#### GitHub
搭建博客最简单快速的方式就是使用 [GitHub Pages](https://pages.github.com/)，但国内访问 github 的速度😂；可能你会说 Coding Pages在国内访问快，除了广告之外好像也没啥缺点，大伙也可以试试，但是这样就没折腾的乐趣了。

我们把 github 仅仅作为博客的托管仓库，不使用它的 GitHub Pages部署，只要想个办法加速访问就可以了。

#### Netlify

[Netlify](https://www.netlify.com/) 是一个提供静态网站托管的服务，提供 CI 服务，能够将托管 GitHub，GitLab 等网站上的 Jekyll，Hexo，Hugo 等静态网站。使用也非常简单。

* 可以托管静态资源
* 可以将静态网站部署到 CDN 上,加速国内访问
* Continuous Deployment 持续部署,当你提交改变到 git 仓库,它就会自动运行 build command ,进行自动部署
* 可以添加自定义域名
* 可以启用免费的 TLS 证书,启用 HTTPS
* 最强大的cms,用来管理静态资源
* 自带 CI、支持自定义页面重定向、自定义插入代码、打包和压缩 JS 和 CSS、压缩图片、处理图片

#### Cloudflare
Netlify 虽然已经提供了 CDN 加速，但在使用过程中发现国内访问还是比较慢，[Cloudflare](https://www.cloudflare.com/zh-cn/) 相对于国内的七牛云、阿里云等云服务商的 CDN 速度会慢一些，但是它有免费版本，而且最重要的是域名不用备案。

### 动手时间

#### Hexo 生成博客项目

* 安装
```js
npm install -g hexo-cli // or
yarn global add hexo-cli
```
* 初始化
```js
hexo init "博客目录" 
cd "博客目录"         
npm install // or
yarn
```
* 目录结构
```js
.
├── _config.yml
├── package.json
├── scaffolds
│   ├── draft.md
│   ├── page.md
│   └── post.md
├── source
│   └── _posts
└── themes
    └── landscape
```
**_config.yml**

全局配置文件，网站的很多信息都在这里配置，诸如网站名称，副标题，描述，作者，语言，主题，部署等等参数。

```yml
# Site
title: Hexo  # 网站标题
subtitle:    # 网站副标题
description: # 网站描述
author: John Doe  # 作者
language:    # 语言
timezone:    # 网站时区。Hexo 默认使用您电脑的时区。时区列表。比如说：America/New_York, Japan, 和 UTC 。

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child'
## and root as '/child/'
url: http://yoursite.com   # 你的站点Url
root: /                    # 站点的根目录
permalink: :year/:month/:day/:title/   # 文章的 永久链接 格式   
permalink_defaults:        # 永久链接中各部分的默认值

# Directory   
source_dir: source     # 资源文件夹，这个文件夹用来存放内容
public_dir: public     # 公共文件夹，这个文件夹用于存放生成的站点文件。
tag_dir: tags          # 标签文件夹     
archive_dir: archives  # 归档文件夹
category_dir: categories     # 分类文件夹
code_dir: downloads/code     # Include code 文件夹
i18n_dir: :lang              # 国际化（i18n）文件夹
skip_render:                 # 跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。    

# Writing
new_post_name: :title.md  # 新文章的文件名称
default_layout: post      # 预设布局
titlecase: false          # 把标题转换为 title case
external_link: true       # 在新标签中打开链接
filename_case: 0          # 把文件名称转换为 (1) 小写或 (2) 大写
render_drafts: false      # 是否显示草稿
post_asset_folder: false  # 是否启动 Asset 文件夹
relative_link: false      # 把链接改为与根目录的相对位址    
future: true              # 显示未来的文章
highlight:                # 内容中代码块的设置    
  enable: true            # 开启代码块高亮
  line_number: true       # 显示行数
  auto_detect: false      # 如果未指定语言，则启用自动检测
  tab_replace:            # 用 n 个空格替换 tabs；如果值为空，则不会替换 tabs

# Category & Tag
default_category: uncategorized
category_map:       # 分类别名
tag_map:            # 标签别名

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD     # 日期格式
time_format: HH:mm:ss       # 时间格式    

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10           # 分页数量
pagination_dir: page   # 分页目录

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape   # 主题名称

# Deployment
## Docs: https://hexo.io/docs/deployment.html
#  部署部分的设置
deploy:     
  type:  # 类型，常用的git 
```
可查看[官网](https://hexo.io/zh-cn/docs/configuration.html)详细配置。

**scaffolds**

scaffolds是“脚手架、骨架”的意思，当你新建一篇文章（hexo new 'title'）的时候，hexo是根据这个目录下的文件进行构建的。基本不用关心。

**source**

这个目录很重要，新建的文章都是在保存在这个目录下的.
_posts 。需要新建的博文都放在 _posts 目录下。

_posts 目录下是一个个 markdown 文件。你应该可以看到一个 hello-world.md 的文件，文章就在这个文件中编辑。

_posts 目录下的md文件，会被编译成html文件，放到 public （此文件现在应该没有，因为你还没有编译过）文件夹下。

**themes**

网站主题目录，hexo有非常好的主题拓展，支持的主题也很丰富。该目录下，每一个子目录就是一个主题。

* 生成文章
```js
hexo new post "第一篇博客" // 会在 source/_posts/ 目录下生成文件 ‘第一篇博客.md’，打开编辑
hexo g // 生成静态HTML文件到 /public 文件夹中
hexo s // 本地运行server服务预览，打开 http://localhost:4000 即可预览你的博客
```
> 可查看官网详细的 [CLI](https://hexo.io/zh-cn/docs/commands) 命令

#### 添加 npm script
```json
// package.json
scripts: {
  "build": "hexo generate", 
  "clean": "hexo clean",
  "server": "hexo server",
  "netlify": "npm run clean && npm run build"
}
```
#### 托管到 Github
* 创建仓库 | Create a new repository
* 本地项目推送到远程服务器
```
git init
git add .
git commit -m "my blog first commit"
git remote add origin git@github.com:【你的 github 名字】/【你的 repository 名字】.git
git push -u origin master
```
#### 连接 Netlify
* 注册 [Netlify](https://www.netlify.com/) 并登陆
* 创建 site

![](https://user-gold-cdn.xitu.io/2020/4/12/1716e180d6718091?w=2462&h=878&f=png&s=103513)

* 连接 github
 
![](https://user-gold-cdn.xitu.io/2020/4/12/1716e18adc5e4c7f?w=2430&h=1276&f=png&s=156148)

* 选择刚刚上传的项目

![](https://user-gold-cdn.xitu.io/2020/4/12/1716e19d3ae96aeb?w=2442&h=1432&f=png&s=200574)

* 选择构建分支、构建命令和静态文件目录，点击 Deploy site 发布

![](https://user-gold-cdn.xitu.io/2020/4/12/1716e1ca7849cacd?w=1794&h=1840&f=png&s=201688)

* 等构建结束后，会看到这里有个网址，打开就是你的博客了

![](https://user-gold-cdn.xitu.io/2020/4/12/1716e249388b99b0?w=2472&h=794&f=png&s=124685)

* 可以点击下方 Domain settings，自定义二级域名

![](https://user-gold-cdn.xitu.io/2020/4/12/1716e25dbcb09f6e?w=2542&h=1010&f=png&s=172372)

* 可以欣赏你的博客了

之后每次对 master 分支提交 commit 时都会触发 Netlify 自动构建。

很多博文都让大家安装 hexo-deployer-git 这个依赖包，这个包把 public 文件夹提交到单独的分支，然后 Netlify 绑定到这个分支，不需要 Netlify构建，完全把 Netlify 当做静态文件托管，把 CI 功能舍弃掉了。

#### 买域名
* 购买域名
* 配置域名解析

![](https://user-gold-cdn.xitu.io/2020/4/12/1716e313e9bb33c7?w=3352&h=1266&f=png&s=375481)

* 按照下图设置 CNAME， 指向 Netlify 里设置的域名

![](https://user-gold-cdn.xitu.io/2020/4/12/1716e310fdcf2dcb?w=2916&h=812&f=png&s=174761)

* 稍等会就可以正常访问了

#### Cloudflare 加速
* 注册 [Cloudflare](https://www.cloudflare.com/zh-cn/) 并登陆
* 添加站点

![](https://user-gold-cdn.xitu.io/2020/4/12/1716e88697bac2cd?w=1888&h=568&f=png&s=62298)
* 选择免费套餐
* 添加 DNS 记录

![](https://user-gold-cdn.xitu.io/2020/4/12/1716e8a970e71352?w=1988&h=1738&f=png&s=288025)

**坑 1：**

>**知识补充1**：我们在上面提到域名解析的时候已经提到 CNAME 类型了，这里又出现了 A 和 AAAA，除此之外还有很多其他类型，有兴趣可以查资料了解下。
DNS 的作用就是把域名翻译成 IP 地址，所以一般在做域名解析的时候，需要添加A（IPV4）或者AAAA（IPV6），把域名指向 IP 地址。但有的时候会需要设置多个域名指向同一个IP地址的情况，这时我们就可以把其他的域名的解析设置成 CNAME 类型，并填写记录值为那个设置了 A 或 AAAA 直接指向IP 地址的域名。这样可以避免服务器 IP变更需要重新设置多个域名解析。直白点说 A 或 AAAA 就是域名直接指向 IP，CNAME 就是域名指向另一个域名。

一般情况下 Cloudflare 会检测出来几条 DNS 记录，类型大多数是A，或者AAAA，由于我们是转发，所以应该是 CNAME 类型才对。所以全部删除，手动添加。

![](https://user-gold-cdn.xitu.io/2020/4/12/1716e9c9e62501c5?w=2118&h=768&f=png&s=120496)

如图，把名称替换成你的域名，内容填你的博客的 Netlify 地址。 www 的名称不用修改，修改内容同上。

>**知识补充2**：CDN 加速往往有两种方式，一种是 CDN 服务商提供一个域名，然后设置你的域名 CNAME 指向这个加速域名；另一种是CND服务商提供 几个NS（DNS解析服务器）地址，然后覆盖你的域名的 NS 地址。Cloudflare就是采用的后者

* 修改 NS


![](https://user-gold-cdn.xitu.io/2020/4/12/1716ea4095323100)

在你的域名服务商那里修改 dns 解析服务器为 cloudflare 提供的地址，修改完成后点击完成。

* 设置强制 HTTPS，文件压缩等

![](https://user-gold-cdn.xitu.io/2020/4/12/1716ea63ab1ed810?w=1908&h=1752&f=png&s=257362)

* 点击完成，设置完毕，NS 方式修改的生效时间会比较长一些，官网上说24小时以内，不过一般不会这么长时间，耐心等待下。成功之后Cloudflare会发一封邮件给你，或者重新登录控制台也能看到。

* 检验成果

返回头部的 server 字段从 Netlify 变成了 cloudflare，速度应该快很多了。

![](https://user-gold-cdn.xitu.io/2020/4/12/1716eabfa7ef57ff?w=1636&h=950&f=png&s=228482)

🎉恭喜，你的博客已经搭建完毕，现在可以开始写文章了。

### 写文

#### 换主题 | Theme
hexo 默认的主题样式欣赏不来，[社区](https://hexo.io/themes/)里有很多主题可以下载。我使用的是简约的 next 主题。

设置步骤如下：

* GitHub 里 fork 主题项目到你的仓库
* 在你的hexo 项目里执行
```
git submodule add git@github.com:【fork的项目】.git themes/【主题的名字，例如 next】
```
git submodule 的使用方式可以 google 了解下。
* 修改 根目录 _config.yml 文件的 theme 的值为你使用的主题名字
* 本地看效果
```
npm run netlify
hexo s
```
* 主题的文件夹里也有 _config.yml，这个是主题的配置文件，根据你的喜好自定义。
* 提交 commit 到远程服务器，更新线上博客。

**坑 2：**

打开博客发现并没有更新主题，查看netlify 构建日志发现失败了，提示没有 submodule 的主题项目的权限：

![](https://user-gold-cdn.xitu.io/2020/4/12/1716ec35cf72b711?w=2446&h=1832&f=png&s=398373)

需要添加 Deploy key到 GitHub

![](https://user-gold-cdn.xitu.io/2020/4/12/1716ec402beb3e2b?w=2530&h=1442&f=png&s=272853)

![](https://user-gold-cdn.xitu.io/2020/4/12/1716ec4815969fd5?w=2086&h=1310&f=png&s=329873)

![](https://user-gold-cdn.xitu.io/2020/4/12/1716ec4b12faa419?w=2504&h=1224&f=png&s=210531)
重新构建，成功🎉，线上博客主题已更新。

#### 添加博客评论 | Gitalk

可以参考[此文](https://asdfv1929.github.io/2018/01/20/gitalk/)设置

callback 务必填写你的域名，这样评论里 github 登录后才能正确回跳到你的博客。

![](https://user-gold-cdn.xitu.io/2020/4/12/1716eca1ef56278b?w=1206&h=1180&f=png&s=121840)

**坑 3**

部署到线上后，在你的文章底部就会出现gitalk的组件，但是会发现存在这样的情况：

![](https://user-gold-cdn.xitu.io/2020/4/12/1716eceb002d409a?w=2050&h=666&f=png&s=65234)

有的会报错 <span style="color: red">network error</span>。

原因如下：

1. hexo 的全局配置 _config.yml 里关于文章 url 路径的配置是这样：
```
permalink: :year/:month/:day/:title/
```
如果你的文章 title 是中文，这会导致你的文章地址栏的地址很长。这会带来两个问题，第一如果修改了文章 title，原来的地址就失效了，SEO 也没了。第二就是 gitalk 需要在 GitHub中创建 issue，而 issue 的 label 长度必须小于 50，而 gitalk 会拿地址栏的 path 作为 label，所以会导致 gitalk 连接失败。

使用 hexo-abbrlink 插件，解决 path 超出：

``` 
npm i hexo-abbrlink --save-dev
```
修改_config.yml
```yml
# abbrlink config
abbrlink:
  alg: crc16  #support crc16(default) and crc32
  rep: hex    #support dec(default) and hex

# 更改 permalink 值
permalink: p/:abbrlink.html 
```
重新运行静态构建
```
npm run netlify
```
生成的文章 url 将会是这样的：
```
https://yourwebsite.com/p/37232.html
```
放心，之后每次 generate 都不会变更。

2. 还需要为每一篇文章新增 issue，这样才能初始化评论。

* 在你的 GitHub 博客项目中添加 issue

* ![](https://user-gold-cdn.xitu.io/2020/4/12/1716edebf66977b0?w=1986&h=856&f=png&s=161608)

* 添加 label 为 Gitalk 和你的文章path（比如/p/37232）

![](https://user-gold-cdn.xitu.io/2020/4/12/1716ee2051589c33?w=2140&h=1570&f=png&s=295948)

* 然后重新提交commit，等待 Netlify 构建完毕，查看你的文章底部 Gitalk 是否已经可以评论

![](https://user-gold-cdn.xitu.io/2020/4/12/1716ee3e9e9b035b?w=2216&h=836&f=png&s=91071)

### 完结

以上就是全部手摸手搭博客教程，大家如果按照步骤，应该已经完成搭建了，hexo 和 netlify 还有很多好玩的功能，可以去玩玩看，我还在摸索中😵，欢迎交流。

所谓万事开头难，其实不然，最难的是坚持，这是我的第二篇文章，之后还会继续写下去，也期待你的加入，与君共勉😁。

「Stay Hungry, Stay Foolish.」

文中若有错误或补充，请不吝赐教。
