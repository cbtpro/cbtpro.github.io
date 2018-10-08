---
layout: post
title:  "安装protractor报DNS错误"
date:   2017-02-09 23:57:50 +0800
categories: jekyll update
---

protractor是一个用于angularjs的自动化段对端测试运行工具,可以模拟用户交互,帮程序员验证angularjs的运行状况.
首先安装protractor

```
npm install -g protractor
```

然后更新webdriver-manager

```
webdriver-manager update
```

在执行这句命令的时候会报一个Hostname/IP doesn't match certificate's altnames的错误,猜测因为在后台下载[http://selenium-release.storage.googleapis.com/](http://selenium-release.storage.googleapis.com/)的时候在域名解析器里找不到正确的地址.而这个地址列出了selenium的发行包.

直接执行启动命令

```
webdriver-manager start
```

可以看到报了个selenium是必须的部分.配置DNS服务地址为谷歌的DNS 8.8..8.8和8.8.4.4,再重新执行命令就可以了.

![img](http://echo.chenbitao.com/googlednsconfig.png)