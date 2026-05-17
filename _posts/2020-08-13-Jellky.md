---
layout: post
author: cbtpro
---
# 安装
```shell
  # 安装jekyll
  $gem install jekyll bundler
  # 创建工程
  $jekyll new my-awesome-site
  # 切换到工程目录
  $cd my-awesome-site
  # 
  $bundle install
  # 启动服务
  $bundle exec jekyll serve

  # => 打开浏览器 http://localhost:4000

```
# 新建菜单

在根目录新建一个md问，在头部指定

```
---
layout: "page"
title: 捐助
permalink: /donate/
---

Donate to our site
```
# 使用permalink固定文章链接
```
premalink: /react/redux
```
premalink的其他用法
```
premalink: /:categories/:year/:month/:day/:title.html
# or
premalink: /:categories/:year/:month/:day/:title.php
# or
premalink: /:categories/:year/:month/:day/:title.jsp
# or
premalink: /:categories/:year/:month/:day/:title
```

# 草稿
在草稿中请不要指定日期

# [主题](https://rubygems.org/search?utf8=%E2%9C%93&query=minima)
