# gh-pages发布静态页面到GitHub-pages

[gh-pages](https://www.npmjs.com/package/gh-pages)是基于nodejs开发的[github pages](https://docs.github.com/cn/github/working-with-github-pages)发布工具，可以将任何文件发布到指定的github的gh-pages仓库下。

github-pages基于jekyll，资源目录要求放在`assets`目录下。

> 除`index.html`和`index.md`外，其余的资源文如javascript文件、style文件等，必须放在assets目录下，其他目录不会被托管。详情请参看[jekyll文档](http://jekyllcn.com/docs/static-files/)

## 发布

### 基于命令行

使用方法gh-pages -d 待发布的目录 --repo 目标仓库

```shell
gh-pages -d dist --repo https://github.com/example/example-demo.git
Published
```

### 基于package.json

如果是一个node.js项目，在构建代码后，可以使用`gh-pages -d 构建后的目录`直接发布项目，它会直接从package.json中获取仓库的git地址。

```shell
# dist是构建后的目录
gh-pages -d dist
```

## 权限

如果操作频繁，可以配置公钥来减少输入密码的操作

## 访问

当发布成功后，会输出`Published`字样后，稍等片刻，即可通过`<user>.github.io/example`访问生效后的代码。



相关文档

gh-pages

https://www.npmjs.com/package/gh-pages

github-pages

https://docs.github.com/cn/github/working-with-github-pages

