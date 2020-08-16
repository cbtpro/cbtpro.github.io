---
layout: post
title:  "安装Apache Sever"
categories: jekyll update
---

我们要运行自己的应用就必须先安装一个容器，先从ApacheServer开始吧

可以从bing上搜索Apahce的官网，或者直接访问[http://httpd.apache.org/download.cgi](http://httpd.apache.org/download.cgi)来下载。

Apache HTTP Server X.X.XX(httpd) 字样的就是要下载的Apache，选择最近版本，会跳到下载的锚点部分，如果是如果是linux，选择.tar.bz2或者tar.gzde 包。这里因为操作系统是Window，所以选择Files for Microsoft Windows.当新手打开这个页面的时候肯定会蒙圈：打开的页面就是没有exe或者msi的安装包。这是因为Apache本身不提供已编译的安装包，只提供源码，如果自己无法编译，可以选择官方推荐的第三方提供的编译网站。

[![QQ截图20160129230345](http://echo.chenbitao.com/wp-content/uploads/2016/01/QQ截图20160129230345.png)](http://echo.chenbitao.com/wp-content/uploads/2016/01/QQ截图20160129230345.png)  我们用ApacheHaus来做示范。

ApacheHaus列出了各个版本，包括各种组件、升级包。选择Apache 2.4 VC11下载，下载完成后解压，将Apache24放到你想要放的地方，接下来就开始安装了。

安装先决条件：

1.  修改Apache24/conf/httpd.conf,将**Define SRVROOT "/Apache24"**修改成你的Apache24的目录下，我这里是**Define SRVROOT "C:\developer\container\Apache24"**
2.  以管理员身份运行cmd.exe，进入到Apache24下的bin目录。
开始安装：

1.  执行**httpd -k install**
2.  会输出[error]Apache2.4: Service is already install.
这个时候你检查你的服务，会发现多出了一个Apache24的服务项，可以像普通服务一样启动和停止。

启动后可以通过http://localhost来打开Apache的首页检查是否启动成功。

开始享受Apache给你带来的热情服务吧！

补充：

在Apache24/bin下有个ApacheMonitor，可以可视化的对Apache进行管理。

注意apache的端口默认是80,如果启动失败，用netstat -aon|findstr "80"来检测80端口是否被占用。

也可以修改apache/conf/httpd.conf修改Listen 80的数字部分来修改默认端口，修改后保存重启即可生效。

停止服务使用**httpd -k install**或者**httpd -k shutdown**

执行安装命令时，如果提示缺失组件或者报任何错误，百度电脑专家是个很好的工具，可以用他来修复，修复完成后继续安装。

[![QQ截图20160129231951](http://echo.chenbitao.com/wp-content/uploads/2016/01/QQ截图20160129231951.png)](http://echo.chenbitao.com/wp-content/uploads/2016/01/QQ截图20160129231951.png)

&nbsp;

&nbsp;

&nbsp;