---
layout: post
title:  "Mac 使用Apache和PHP"
date:   2016-02-19 17:46:37 +0800
categories: jekyll update
---

Mac OS X内置了Apache和PHP，所以我们只需要简单的配置就可以使用PHP。我这里以Mac OS X EI Capitan 版本10.11.3为例子。

下面的操作都是在终端中操作：

启动apache服务:
```shell
sudo apachectl start
```

显示apache版本:
```shell
sudo apachectl -v
```

停止apache服务:
```shell
sudo apachectl stop
```

启动服务后，可以在地址栏输入localhost或者ip地址来访问，端口是80.显示it work!表示apache服务启动。

apache的默认根目录在/Library/WebServer/Documents下，可以在Finder里输入Shift + commond + G来访问指定目录。

配置php：

开启php：sudo vi /etc/apache2/httpd.conf，将'#LoadModule php5_module libexec/apache2/libphp5.so'前的井号删除掉，wq保存。

[![QQ20160219-0@2x](/assets/images/2016/02/QQ20160219-0@2x.png)](/assets/images/2016/02/QQ20160219-0@2x.png)

复制php配置文件：
```shell
sudo cp /etc/php.ini.default /etc/php.ini
```

此时php功能已经打开，可以在 /Library/WebServer/Documents目录下新建一个phptest.php文件，来测试能否正常访问php文件，文件内容如下：

[![QQ20160219-2@2x](/assets/images/2016/02/QQ20160219-2@2x.png)](/assets/images/2016/02/QQ20160219-2@2x.png)

&nbsp;

显示以下内容则PHP开启完成

[![QQ20160219-3@2x](/assets/images/2016/02/QQ20160219-3@2x.png)](/assets/images/2016/02/QQ20160219-3@2x.png)
