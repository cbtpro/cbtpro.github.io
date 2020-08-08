# github发布静态博客

## jekyll

jekyll是github官方推荐使用的静态博客工具，有非常丰富的[中文文档](http://jekyllcn.com/)，下面只列出几点需要注意的和技巧

### 安装[Ruby](https://www.ruby-lang.org/zh_cn/downloads/)

ruby地址需要科学上网使用，如果您没有科学上网，这里已经准备好了一个地址，直接下载安装使用即可。

源码安装https://ruby-china.org/wiki/ruby-mirror

### 设置ruby镜像地址

参考https://gems.ruby-china.com/中设置国内地址镜像的操作，如果网络不好，这一步会大大加速gem的速度。

### 支持主题功能

## Hugo

[hugo](https://www.gohugo.org/)是基于golang开发的静态博客工具，原理与jekyll一样，都是将使用模版引擎将博客编译成静态文件交给github-pages托管。

### 支持皮肤功能

## hexo

[hexo](https://hexo.io/zh-cn/docs/)是基于nodejs开发的静态博客工具，原理与jekyll一样，都是使用模版引擎将博客编译成静态文件交给github-pages托管。