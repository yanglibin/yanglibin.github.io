# HugoBlog搭建与使用


<!--more-->

<!-- Place resource files in the current article directory and reference them using relative paths, like this: `![alt](images/screenshot.jpg)`. -->


[Hugo](https://gohugo.io/) 是用 Go 实现的博客工具



## 1.安装 hugo 命令工具

安装详情请参考[官方文档](https://hugo.opendocs.io/installation/windows/)

先决条件:

- go 语言环境
- git

```bash
1. 从 github.com/gohugoio/hugo@latest 下载 hugo_extended 版本，如:hugo_extended_0.148.2_windows-amd64.zip
2. 将hugo.ext 解压到 $GOPATH/bin
```



## 2. 创建一个新的站点目录

```bash
## 在blog 存放的位置运行下面的命令:
hugo new site ekcool_blog_site

Congratulations! Your new Hugo site was created in C:\mybook\myblog\ekcool_blog_site.

Just a few more steps...

1. Change the current directory to C:\mybook\myblog\ekcool_blog_site.
2. Create or install a theme:
   - Create a new theme with the command "hugo new theme <THEMENAME>"
   - Or, install a theme from https://themes.gohugo.io/
3. Edit hugo.toml, setting the "theme" property to the theme name.
4. Create new content with the command "hugo new content <SECTIONNAME>\<FILENAME>.<FORMAT>".
5. Start the embedded web server with the command "hugo server --buildDrafts".

See documentation at https://gohugo.io/.


## 创建完成后，根目录 ekcool_blog_site 包含以下文件
.
├── archetypes: default.md是生成博文的模版
├── assets     # 存放被 Hugo Pipes 处理的文件
├── content    # 存放markdown文件作为博文内容
├── data       # 存放 Hugo 处理的数据
├── layouts    # 存放布局文件
├── static     # 存放静态文件 图片 CSS JS文件
├── themes:    # 存放不同的主题
└── hugo.toml: 博客配置文件支持 JSON YAML TOML 三种格式配置文件


```



## 3. 配置主题

官方主题网站: https://themes.gohugo.io/，我们这里选择[`FixIt`](https://github.com/hugo-fixit/FixIt) 主题，主要是它支持二级菜单。

```bash
## https://github.com/hugo-fixit/FixIt  # 支持二级目录

cd ekcool_blog_site
## git clone https://github.com/yanglibin/hugo-PaperMod.git themes/hugo-PaperMod
git init
git submodule add https://github.com/hugo-fixit/FixIt.git themes/FixIt
echo "theme = 'FixIt'" >> hugo.toml
echo "defaultContentLanguage = 'zh-cn'" >> hugo.toml

## 为了能完整地使用 FixIt 主题的所有功能，务必在站点配置文件中添加以下内容。
[markup]
  _merge = "shallow"
[outputs]
  _merge = "shallow"
[taxonomies]
  _merge = "shallow"
```

因为个人偏爱 `yml` 的阅读习惯，所以本文使用 `yml` 而非默认的 `toml` 文件类型来配置，大家可以根据偏好到 [transform.tool](https://transform.tools/toml-to-yaml) 进行转换。



```yaml
baseURL: https://blog.ekcool.com
languageCode: en-us
title: ekcool's blog
theme: FixIt
defaultContentLanguage: zh-cn

## 为了能完整地使用 FixIt 主题的所有功能，务必在站点配置文件中添加以下内容
markup:
  _merge: shallow
outputs:
  _merge: shallow
taxonomies:
  _merge: shallow


menu:
  main:
    - identifier: posts
      name: 📝文章 
      url: /posts/
      weight: 10
    - identifier: categories
      name: 🗂️分类
      url: /categories/
      weight: 10
    - identifier: categories_linux
      parent: categories
      name: linux
      url: /categories/linux
      weight: 1
    - identifier: categories_mysql
      parent: categories
      name: mysql
      url: /categories/mysql
      weight: 2
    - identifier: categories_k8s
      parent: categories
      name: k8s
      url: /categories/k8s
      weight: 3

    - identifier: tags
      name: 🏷️标签
      url: /tags/
      weight: 20

    - identifier: search
      name: 🔍search
      url: /search/
      weight: 40
    - identifier: github
      name: github
      url: https://github.com/
      weight: 40

    - identifier: links
      name: 🤝友链
      url: friends
      
      weight: 60

    - identifier: about
      name: 🙋关于我
      url: /about/
      weight: 10



```



下面是用到的配置文件:

```yaml
baseURL: https://blog.ekcool.com
languageCode: en-us
title: ekcool's blog
theme: hugo-PaperMod
enableRobotsTXT: true
buildDrafts: false
buildFuture: false
buildExpired: false
googleAnalytics: UA-123-45
pygmentsUseClasses: true
minify:
  disableXML: true
  minifyOutput: true
params:

  env: production # to enable google analytics, opengraph, twitter-cards and schema.
  description: "这是一个纯粹的博客......"
  author: Libin Yang
  
  defaultTheme: auto  # defaultTheme: light or  dark
  disableThemeToggle: false
  DateFormat: "2006-01-02"  # 时间格式

  # disableSpecialistPost: true
  displayFullLangName: true


  ShowCodeCopyButtons: true
  hideFooter: false # 隐藏页脚
  ShowWordCounts: true  # 显示字数
  VisitCount: true
  
  ShowBreadCrumbs: true # 面包屑导航
  ShowReadingTime: true # 阅读时间
  ShowShareButtons: true # 分享按钮
  ShowPostNavLinks: true # 文章前后页按钮

  ShowToc: true # 显示目录
  TocOpen: true # 自动展开目录
  ShowLastMod: true #显示文章更新时间
  comments: true  # 开启评论


  label:
    text: Home 
    icon: /apple-touch-icon.png
    iconHeight: 35
###############    首页显示配置   ##########################
  # 将 Index/Home 显示为带有社交链接和图像的完整页面
  profileMode:
    enabled: false
    title: 👋欢迎光临👋
    subtitle: 这个家伙很懒，什么都没留下
    imageUrl: https://nsddd.top/assets/cubxxw-image_hu6453afe1ffa3ceac1e2ec46949831dee_17358_120x120_resize_q75_box.jpg
    imageWidth: 120
    imageHeight: 120
    imageTitle: my image
    # buttons:
    #   - name: Archive
    #     url: "/archive"
    #   - name: Github
    #     url: "https://github.com/"
    #   - name: Posts
    #     url: posts
    #   - name: Tags
    #     url: tags

  homeInfoParams:  # 常规模式（默认模式）
    Title:  👋欢迎光临👋
    Content: Welcome to my blog

  # 首页社交账号
  # socialIcons:
  #   - name: twitter
  #     url: https://twitter.com/
  #   - name: stackoverflow
  #     url: https://stackoverflow.com
  #   - name: github
  #     url: https://github.com/

############################################################
  # analytics:
  #   google:
  #     SiteVerificationTag: XYZabc
  #   bing:
  #     SiteVerificationTag: XYZabc
  #   yandex:
  #     SiteVerificationTag: XYZabc
  # cover:
  #   hidden: true
  #   hiddenInList: true
  #   hiddenInSingle: true
  # editPost:
  #   URL: https://github.com/<path_to_repo>/content
  #   Text: Suggest Changes
  #   appendFilePath: true
  # fuseOpts:
  #   isCaseSensitive: false
  #   shouldSort: true
  #   location: 0
  #   distance: 1000
  #   threshold: 0.4
  #   minMatchCharLength: 0
  #   keys:
  #     - title
  #     - permalink
  #     - summary
  #     - content

##############    菜单配置     #############
menu:
  main:
    - identifier: posts
      name: 📝文章 
      url: /posts/
      weight: 10
    - identifier: categories
      name: 🗂️分类
      url: /categories/
      weight: 10
    - identifier: tags
      name: 🏷️标签
      url: /tags/
      weight: 20
    # - identifier: others
    #   name: others
    #   url: /others/
    #   weight: 30
    - identifier: search
      name: 🔍search
      url: /search/
      weight: 40
    - identifier: github
      name: github
      url: https://github.com/
      weight: 40

    - identifier: links
      name: 🤝友链
      url: links
      weight: 60

    - identifier: about
      name: 🙋关于我
      url: /about/
      weight: 10
###########################################

## markup:
##   highlight:
##     noClasses: false

outputs:
  home:
    - HTML
    - RSS
    - JSON 
```





### 菜单配置说明

```yaml
menu
  main
    identifier : ""
    #  父级菜单项的标识符 (identifier)
    parent : ""
    # 你可以在名称（允许 HTML 格式）之前添加其他信息，例如图标
    pre : ""
    # 你可以在名称（允许 HTML 格式）之后添加其他信息，例如图标
    post : ""
    name : ""
    url : ""
    # 当你将鼠标悬停在此菜单链接上时，将显示的标题
    title : ""
    weight : 1
    #  向菜单项添加用户定义的内容
    params
      # 添加 CSS 类到菜单项
      class : ""
      # 是否为草稿菜单，类似草稿页面
      draft : false
      #  添加 fontawesome 图标到菜单项
      icon : ""
      #  设置菜单项类型，可选值："mobile", "desktop"
      type : ""
      #  是否显示子菜单项分割线
      divided : false
```

### 主题配置说明

除了 Hugo 全局配置外，FixIt 还通过根配置键 `params` 提供了一些主题配置。

一个简单的例子：

```toml
baseURL = 'https://example.org/'
languageCode = 'en'
title = 'ABC Widgets, Inc.'
[params]
  version = "0.3.X"
  description = "This is my new Hugo FixIt site"
  keywords = ["Hugo", "FixIt"]
  # ...
  
```

[配置详情介绍](https://fixit.lruihao.cn/zh-cn/documentation/getting-started/configuration/#theme-configuration)



## 4. 文章发布



### 常用操作命令

```bash
## 使用默认模板 ./themes/FixIt/archetypes/post-bundle/index.md, 在content/posts 中创建文章
hugo new posts/test1.md  # 会在 content 下面创建一个 posts目录,生成 test.md 文件

## 使用默认模板, 在content/others 中创建文章
hugo new others/test2.md 

## 使用模板post.md 在content/others 中创建文章。  通过 --kink 指定模板名称
hugo new --kind post content/others/aaa.md

hugo server -D
## 上述命令会生成所有的页面，并把它们放在 public/ 目录中

## 由于主题使用了 Hugo 中的 .Store 来实现一些特性， 非常建议你为 hugo server 命令添加 --disableFastRender 参数来实时预览你正在编辑的文章页面
hugo server --disableFastRender
```



###  文章模板

Hugo 提供了一个默认的文章使用模板，位于`archetypes/default.md`，里面包括一些基本内容标题、日期以及是否为草稿：

为方便写作，我们可以在文件夹 `archetypes` 中创建多个模板文件，如`post.md` 文件， 下面是模板内容：

```
---
date: "{{ .Date }}"
draft: false       # 草稿标识, 需要改为 false 才能发表博文
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
tags:
  - tags1
  - tags2
categories: 
  - categories1
keywords: 
  - key1
weight:            # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
summary: ""        # 文章简单描述，会展示在主页
description: ""    # 文章描述，与搜索优化相关

hidemeta: false    # 是否隐藏文章的元信息，如发布日期、作者等

---

<!-- more -->
```



```bash
## 使用 模板post.md 创建文章。  通过 --kink 指定模板名称
hugo new --kind post content/others/aaa.md
```



### 文章中使用图片

一般情况下页面资源使用 [`页面包`](https://hugo.opendocs.io/content-management/page-resources/) 来管理



我们先讲下Hugo的一个实现逻辑：

1. Hugo博客的根目录有一个`static`目录，这个static目录就是用来存放一些静态文件，比如图片、css、js文件等。
2. 执行`hugo`命令的时候，会把`static`目录下的子目录或文件复制到`public`目录下。比如我在`static`下添加了一个`img`子目录，并且在`img`子目录放了图片，那执行`hugo`命令后，就会把`static\img`文件的内容拷贝到`public\img`里面。
3. Hugo博客网站展示的其实是`public`下的内容，因此markdown文章里引用图片的时候，得引用pubic下的图片才可以。



具体操作非常简单，分2步：

1. 在`static`目录下创建`img`子目录，把markdown要使用的图片放在`static\img`目录里。
2. 在markdown文件里，按照如下格式`![](/img/abc.jpg)` 引用图片(这里假设图片名称叫abc.jpg)。这样最终public目录下生成的静态页面就可以引用到`public\img`下的图片了。



实际在文章中使用:

```mark
![abc图片](/img/abc.jpg)
```



###  文章链接

> 通过链接，给系列文章制作目录页面

通常情况下在`/content/posts` 下面创建一下系列文件夹，如: `k8s`




#### Relref

尽量使用此种方式

````markdown
```
- [Hugo使用Github Action自动部署博客到Github Pages]({{% relref "/posts/hugo_blog_githubActionDeploy" %}})
```
````
效果如下:
- [Hugo使用Github Action自动部署博客到Github Pages]({{% relref "/posts/hugo_blog_githubActionDeploy" %}})

## 5. 页面优化

> 为了防止更新主题时还原修改过的文件，需要在网站根目录进行修改，这样可以覆盖主题原有文件的效果。



### about页面

```bash
hugo new --kind post content/about.md
## 编辑 about.md 即可
```



在config.yml里加入

```yaml
menu:
  main:
    - identifier: about
      name: 🙋关于我
      url: /about/
      weight: 10
```



### [添加搜索功能](https://fixit.lruihao.cn/zh-cn/guides/cse-support/)



首先，手动创建一个搜索结果展示页面：

```bash
hugo new content search/index.md
```

在 FixIt 主题中配置 Google 或 Bing CSE：

```yaml
params:
  search:
    enable: true
    type: cse
  cse:
    engine: google
    resultsPage: /search/
    google:
      cx: ""
    bing: {}  # 暂不支持

```

Google CSE 又叫 Google Programmable Search Engine, 本节主要介绍如何在 FixIt 中使用 Google CSE，你可前往 [Programmable Search Engine Tutorial](https://developers.google.com/custom-search/docs/overview) 查看完整的教程。

接着，按照以下步骤创建一个 Google 搜索引擎：

- 在 [Google Programmable Search Engine](https://programmablesearchengine.google.com/) 创建一个新的搜索引擎。


- 输入你的搜索引擎名称，例如：`ekcoolBlog`。

  

- 选择在特定网站或网页中搜索，输入你的网站 URL，例如：`blog.ekcool.com`。

  

- 点击 `创建` 按钮。

  

- 稍等片刻，页面会提示你：“新的搜索引擎已创建”，然后点击 `自定义` 按钮进入搜索引擎的自定义页面。

  

- **点击 外观和风格 => 布局 => 选择 `两列`**。（这一步至关重要，请勿忽略！）

  

- 最后点击概览，复制你的搜索引擎 ID。

  

- 将搜索引擎 ID 填入配置文件中的 `cx` 字段。

  

- 🎉 大功告成！重新启动 Hugo 服务器试试搜索功能吧。



**Bing CSE 在 FixIt 中暂不支持。**



### 代码块扩展语法 - 时间线（Timeline）

https://fixit.lruihao.cn/zh-cn/documentation/content-management/timeline-support/



### 添加归档时间轴

在config.yml里加入

```yaml
menu:
  main:
  - identifier: archives
    name: 归档
    url: /archives/
    weight: 10
```

然后再创建一个归档页面:

`hugo new content archives/index.md`



### 添加友情链接

在config.yml里加入

```yaml
menu:
  main:
    - identifier: friends
      name: 🤝友链
      url: friends
      weight: 60
```

然后再创建一个 friend 页面:

```
hugo new content friends/index.md
```

在 Front matter 中设置 `layout: friends`，并在 `yourSite/data/` 目录下创建 `friends.yml`，其内容格式如下：

```yaml
## 朋友/站点信息例子
- nickname: 朋友名字
  avatar: 朋友头像
  url: 站点链接
  description: 对朋友或其站点的说明
```



### 动态花瓣



```bash
cp ./themes/FixIt/layouts/baseof.html ./layouts/


## 在 ./layouts/baseof.html 中的 main 层(第29行) 加入如下内容:
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/Ukenn2112/UkennWeb@3.0/index/web.js"></script>
```



### 内容加密

FixIt 主题提供了两个 front matter 用于全文加密。

- **password**: *[必需]* 加密页面内容的密码
- **message**: *[可选]* 加密提示信息

```yaml
title: 主题文档 - 内容加密
date: 2022-05-28T11:51:41+08:00
author:
  name: yanglibin
  link: 
description: 了解如何在 FixIt 主题中加密内容。
password: 1212
message: 密码是 1212
resources:
  - name: featured-image
    src: featured-image.png
tags:
  - Encryption
categories:
  - Documentation
```



### 添加评论功能 (待续)

```
评论	Twikoo https://twikoo.js.org/
```



## 6.高级功能

可以参考[shortcodes 这里](https://fixit.lruihao.cn/zh-cn/documentation/content-management/shortcodes/extended/)



### 文章中添加音乐

在文章中插入音乐

```
单曲：（autoplay参数为是否自动播放音乐）
{{< music server="netease" type="song" id="1372188635" autoplay="false" >}}
列表：
{{< music auto="https://music.163.com/playlist?id=6843891038">}}

```



### 文章中插入视频

#### youtube视频

> 默认是支持 youtube 视频的

在文章中直接加入如下内容即可:

```
{{< youtube id="CMtN_LUpeLg" >}}
```

#### 支持bilibili

在文章中直接加入如下内容即可:

```
{{< bilibili BV1SVbtzEE2r >}}
或者
{{< bilibili id=BV19S411c7Wu >}}

```

## 参考:
[FixIt 主题官方文档](https://fixit.lruihao.cn/zh-cn/documentation/)


---

> 作者: yanglibin  
> URL: https://blog.ekcool.com/posts/1822896/  

