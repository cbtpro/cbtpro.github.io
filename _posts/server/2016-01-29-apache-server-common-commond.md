---
layout: post
title:  "Apache Server常用命令"
date:   2016-01-29 23:31:27 +0800
categories: jekyll update
---

继上篇：[安装Apache Server](http://echo.chenbitao.com/?p=41)

启动Apache服务器之前，可以用**httpd -n Apache2.4**来检查配置文件是否正确。

解释一下这里命令的意思，-n表示参数后的是name同一台机器可以安装多个Apache服务，在安装的时候可以在httpd -k install -n 名称来安装多个服务。如果不指定名字，默认操作唯一安装的Apache服务。

常用的操作命令：

httpd -k start

httpd -k stop或httpd -k shutdown

httpd -k

http -k install

http -k uninstall

这里的install和uninstall表示安装成一个服务和卸载掉服务。

还有其他参数，我们后面用到的时候再介绍。