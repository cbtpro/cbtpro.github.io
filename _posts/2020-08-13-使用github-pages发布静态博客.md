---
layout: post
author: cbtpro
---

# 使用github-pages发布静态博客

github提供了[`github-pages`](https://docs.github.com/cn/github/working-with-github-pages),链接中是github-pages的中文文档，这篇文章只做到解释的作用。

github-pages帮助用户发布静态页面，但是不能用做商业目的，并且对存储和流量做了限额

- 代码存储库大小为1GB
- 网站大小不能大于1GB
- 软带宽每月100GB
- 每小时最多只能构建10个版本

并且对使用目的进行了规范

- 禁止违反服务条款或者社区准则中非法或以其他方式禁止的内容和活动

- 暴力或者威胁内容或活动

- 自动批量活动过多（例如发送垃圾邮件）
- 危害github用户或github服务的活动
- 快速致富的内容
- 色情内容
- 歪曲您的身份或这网站的内容。

但是对于个人技术博客/个人简历已经足够用了。

## github-pages

github-pages的原理是在你要使用github-pages功能的git仓库下自动新建一个名为`gh-pages`分支，gh-pages分钟的代码包含静态网站的基本内容，但是目录格式不限于此，形成一个静态网站并托管了这个分支。

- index.html

- 样式文件

- javascript文件
- 404.html
- CNAME
- 其他文件

了解了原理就可以来做一个简单的静态网站了。

## 开启gh-pages

### 创建`<user>.github.io`

要发布用户站点，必须创建名为 `<user>.github.io` 的用户帐户所拥有的仓库。<user>就是你注册的用户，请替换成你的用户名

登陆github后，新创建一个repo，命名为`<user>.github.io`

### 开启GitHub-pages

在仓库界面点击设置进入仓库的设置界面，`https://github.com/<user>/<user>.github.io/settings`(将user替换成自己的用户名)，页面滚动到下面找到`Github Pages`，关于github-pages的设置都在这里设置。

如何你没有提交任何代码、也没有任何分支代码，可以直接`Change Theme`按钮，从列表中选择一个theme（主题），github会帮助新建一个主要分支，并在这个分支下新建一个`index.md`和`_config.yml`,这是jekyll的最小配置。

```yaml
theme: jekyll-theme-minimal
```

如果已经提交了代码，直接在Branch下拉框中选择分支，github-pages会在选择的分支下起作用。Select Folder选择根（root），github会根据分支和你选择的分支下的文件夹来构建jekyll静态页面，并新建一个`ph-pages`分支作为放置编译后的静态文件的分支。

#### githuh-pages根目录

在选择Branch后就可以选择Select Folder，由`/(root)`和`/docs`可供选择，它表示github-pages在哪个目录下起作用。

- /(root)表示github-pages将会在根目录下起作用。
- /docs表示github-pages将会在`/docs`下起作用，需要新建`docs`文件夹。

> 手动在上传`index.md`和`_config.yml`效果和方法1是一样的。当存在jekyll配置文件`_config.yml`就会开启了github-pages后，github会开启jekyll构建静态页面，从配置的路径下寻找`index.md`或`index.html`。

#### CNAME

CNAME文件是用于设置自定义域名的配置，将自己的自定义域名写到CNAME文件中，并放在gh-pages分支配置的根目录下，等待构建成功即可生效。

例如

```shell
www.example.com
```

#### 访问github-pages

使用自定义域名或者`<user>.github.io`即可访问，github-pages设置在每次保存后，github都会自动去构建，构建完成既可以生效，构建过程可以在`https://github.com/<user>/<user>.github.io/deployments`页面看到。

## 子模块

每个用户可以有一个`<user>.github.io`作为主github-pages，当前用户的其他项目也是可以开启github-pages，并且作为子模块来访问。

例如还有一个demo项目也开启了GitHub-pages，则可以通过`<user>.github.io/demo`来访问demo项目下gh-pages的。

## github-pages的妙用

知道github-pages的原理是利用了一个gh-pages的分支存放index.html和其他静态资源即实现托管，可以将任何符合规范的html/js/css代码放到gh-pages分支下，来作为demo、开源项目首页、release包下载等功能。



## github-pages训练课程

https://lab.github.com/githubtraining/github-pages