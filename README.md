# 设置favicon.ico

准备一张至少70x70像素的图片，推荐是260x260像素的图片作为favicon的源文件，访问[realfavicongenerator](https://realfavicongenerator.net/)，然后把图片上传上去，会帮你生成各种设备的favicon.icon，然后按照提示下载，放在你博客的根目录下，在网站的head标签里已经添加了这些favicon的配置。

## 使用gem中国区镜像

```shell
bundle config mirror.https://rubygems.org https://gems.ruby-china.com
```


### 如有必要，使用代理，代理自己提供

```shell
export http_proxy=http://127.0.0.1:49295
export https_proxy=http://127.0.0.1:49295
```

参考 [gems.ruby-china.com/](https://gems.ruby-china.com/)
## 安装依赖

```shell
[sudo] bundle install
```

## 启动

```shell
bundle exec jekyll serve
```