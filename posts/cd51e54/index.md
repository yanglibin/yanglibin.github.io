# Hugo使用Github Action自动部署博客到Github Pages


<!--more-->


将hugo blog 使用github action发布到github page 上.

## 1 准备github仓库



由于我们要使用`github actions`自动部署稍，所以这里我们创建两个仓库.

- `yanglibin.github.io`   : blog展示仓库（公开）

  这个公开的一定要是 `[你的github帐号].github.io`，像我的帐号是`yanglibin.github.io`

- `ekcool_blog_site` :  blog源码存放（私有）



下面来配置ssh 免密:

在你本地使用`ssh-keygen` 命令生成密钥对，然后在github仓库中设置全局 ssh 免密:
  `setting` --> `Access` --> `ssh and GPG keys `，点击`New ssh key` ，将公钥添加上去.



## 2 准备hugo-blog源码

```bash
git clone git@github.com:yanglibin/ekcool_blog_site.git
cd ekcool_blog_site

# 将你创建的hugo 站点目录 copy 到 github 仓库目录下
cp -a [your hugo site dir]/*   ./

# 添加gitignore
cat > ./.gitignore << EOF
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
themes/
public/
.deploy*/
EOF
```

## 3 将站点部署到 github page



hugo手动部署到github pages很简单：

1. 使用`hugo server`生成静态网页
2. 将静态网页push到建好的github pages repo master 分支中，即:`yanglibin/yanglibin.github.io`







- [workflows 官方语法学习](https://docs.github.com/cn/actions/reference/workflow-syntax-for-github-actions)

- [第三方github action 仓库](https://github.com/marketplace?type=actions)



### 3.1. GitHub Actions 简介

大家知道，持续集成由很多操作组成，比如抓取代码、运行测试、登录远程服务器，发布到第三方服务等等。GitHub 把这些操作就称为 actions。

很多操作在不同项目里面是类似的，完全可以共享。GitHub 注意到了这一点，想出了一个很妙的点子，允许开发者把每个操作写成独立的脚本文件，存放到代码仓库，使得其他开发者可以引用。

如果你需要某个 action，不必自己写复杂的脚本，直接引用他人写好的 action 即可，整个持续集成过程，就变成了一个 actions 的组合。这就是 GitHub Actions 最特别的地方。

GitHub 做了一个[官方市场](https://github.com/marketplace?type=actions)，可以搜索到他人提交的 actions。另外，还有一个 [awesome actions](https://github.com/sdras/awesome-actions) 的仓库，也可以找到不少 action。



上面说了，每个 action 就是一个独立脚本，因此可以做成代码仓库，使用`userName/repoName`的语法引用 action。比如，`actions/setup-node`就表示`github.com/actions/setup-node`这个[仓库](https://github.com/actions/setup-node)，它代表一个 action，作用是安装 Node.js。事实上，GitHub 官方的 actions 都放在 [github.com/actions](https://github.com/actions) 里面。

既然 actions 是代码仓库，当然就有版本的概念，用户可以引用某个具体版本的 action。下面都是合法的 action 引用，用的就是 Git 的指针概念，详见[官方文档](https://help.github.com/en/articles/about-actions#versioning-your-action)。

```bash
actions/setup-node@74bc508 # 指向一个 commit
actions/setup-node@v1.0    # 指向一个标签
actions/setup-node@master  # 指向一个分支
```



### 3.2. github workflow

首先，需要获取token

1. 点击右上角GitHub头像 -> 设置 -> 开发者设置 -> Personal access tokens -> Generate new token

   或直接访问`https://github.com/settings/apps`

2. 生成 `Generate new token (classic)`（生成后只显示一次，请复制保存）

   勾选repo里的所有选项， *workflow* 里所有的选项

   还有`admin:repo_hook`

3.  在你的站点仓库:`ekcool_blog_site`: `setting` 中配置变量: 
  - `PERSONAL_TOKEN`   : 上面创建的Token
  - `ACTION_DEPLOY_KEY`: ssh 的私钥，用于在流水线中连接发布仓库

`setting` --> `Secrets and veriables` --> `Actions secrets and variables`，选择`secrets` 标签页，选择`Repository secrets`



```bash
# action workflows
mkdir -p .github/workflows
touch .github/workflows/deploy.yml

# hugo 中引用 文件。 编写 deploy.yml
```



```yaml

# action name
name: hugo Blog Deploy

on:
  push:
    # 触发action分支
    branches:
      - master
#  workflow_dispatch:
#  schedule:
#    # Runs everyday at 8:00 AM
#    - cron: "0 0 * * *"

# 定义变量
env:
  GIT_USER: yanglibin
  GIT_EMAIL: fengyuanyang000@qq.com
  THEME: FixIt
  # https://github.com/hugo-fixit/FixIt.git
  THEME_REPO: hugo-fixit/FixIt
  THEME_BRANCH: main
  THEME_PATH: themes/FixIt


jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # 使用 第三方 Actions  
      # Checkout 当前仓库到本地
      - name: Checkout
        uses: actions/checkout@v2
        with:
           ref: source
      # Checkout 主题
      - name: Checkout theme repo
        uses: actions/checkout@v2
        with:
          repository: ${{ env.THEME_REPO }}
          ref: ${{ env.THEME_BRANCH }}
          path: ${{ env.THEME_PATH }}

      # hugo 环境准备
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
            hugo-version: "latest"
            extended: true
      # hugo build 生成 web页面
      - name: Build Web
        run: hugo

      - name: Deploy Web
        uses: peaceiris/actions-gh-pages@v3
        with:
            #PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
            DEPLOY_KEY: ${{ secrets.ACTION_DEPLOY_KEY }}
            EXTERNAL_REPOSITORY: yanglibin/yanglibin.github.io
            PUBLISH_BRANCH: master
            PUBLISH_DIR: ./public  # 注意这里指的是要发布哪个文件夹的内容，而不是指发布到目的仓库的什么位置，因为hugo默认生成静态网页到public文件夹，所以这里发布public文件夹里的内容
            commit_message: ${{ github.event.head_commit.message }}
```



## 4. push 到github

```bash
git add .
git commit -m "my hugo blog first commit"
git push --set-upstream origin source
```





# 参考:

- [阮一峰-GitHub Actions 入门教程](https://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)

- [Hugo使用Github Action自动部署博客到Github Pages](https://tomial.github.io/posts/hugo%E4%BD%BF%E7%94%A8github-action%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2%E5%8D%9A%E5%AE%A2%E5%88%B0github-pages/)




---

> 作者: yanglibin  
> URL: https://blog.ekcool.com/posts/cd51e54/  

