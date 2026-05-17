---
layout: post
title: "使用 github 的 github pages 功能发布静态网页"
date: 2025-10-12
tags: ["GitHub", "每天一个知识点", "Vue.js", "后端", "程序员", "掘金·日新计划"]
---
## github pages

github pages是 github 提供的静态网页托管工具，一般用于 github 项目介绍。默认可以通过创建一个 gh-pages 的分支来通知 github 来托管，这是约定好的，只要检测到 gh-pages 分支，就会自动进行配置和托管。

然后将开发的 html 页面和资源提交到 gh-pages 分支即可通过`http://你的用户名.github.io/项目目录`来访问 gh-pages 分支的 html 页面。

下面的截图是自动识别后生成的配置。

![image.png](/assets/images/9ed6525c6156.jpg)

当然也可以修改默认配置，比如将 docs 目录进行托管，而不是使用默认的 gh-pages 分支。

## gh-pages

[gh-pages](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fgh-pages)是基于nodejs开发的[github pages](https://link.juejin.cn?target=https%3A%2F%2Fdocs.github.com%2Fcn%2Fgithub%2Fworking-with-github-pages)发布工具，可以将任何文件发布到指定的github的gh-pages仓库下。

### 发布

有时候我们需要将主开发分支的某个目录单独部署到 gh-pages 分支上进行托管，可以使用gh-pages 来进行发布。

### 基于命令行

使用方法gh-pages -d 待发布的目录 --repo 目标仓库

```shell
gh-pages -d dist --repo https://github.com/example/example-demo.git
Published
```

### 基于package.json

如果是一个node.js项目，在构建代码后，可以使用`gh-pages -d 构建后的目录`直接发布项目，它会直接从`package.json`中获取仓库的git的 homepage 配置，如果没有配置 homepage，则会根据项目名称、git 用户名等拼凑出仓库地址。

```shell
# dist是构建后的目录
gh-pages -d dist
```

可以将命令配置到 package.json 中

```json
"scripts": {
    "gh": "gh-pages -d dist"
  },
```

### 权限

如果操作频繁，可以配置公钥来减少输入密码的操作

### 访问

当发布成功后，会输出`Published`字样后，稍等片刻，即可通过`.github.io/example`访问生效后的代码。

## 相关文档

gh-pages

[www.npmjs.com/package/gh-…](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fgh-pages)

github-pages

[docs.github.com/cn/github/w…](https://link.juejin.cn?target=https%3A%2F%2Fdocs.github.com%2Fcn%2Fgithub%2Fworking-with-github-pages)
