---
layout: post
title:  "让Apache支持PHP语言"
date:   2016-01-30 00:49:26 +0800
categories: Apache
---

Apache是一个容器，但是默认是不支持PHP语言的，那么我们需要让Apache能够帮助我们学习和使用PHP。

先从PHP的官网下载PHP的安装包：[http://php.net/downloads.php](http://php.net/downloads.php)

我们用的Window操作系统，那么选择Windows downloads。

[![QQ截图20160129233453](/assets/images/2016/01/QQ截图20160129233453.png)](/assets/images/2016/01/QQ截图20160129233453.png)    一定要下载编译过的安装包，我们以VC14(注1)编译的64位Thread Safe的为例子。

[![QQ截图20160129233814](/assets/images/2016/01/QQ截图20160129233814.png)](/assets/images/2016/01/QQ截图20160129233814.png)

下载解压后，将文件名改成php7，并且复制到你要安装的目录下。我这里放在了C盘根目录。

然后进入到php7目录，将php.ini-development复制一份重命名为php.ini。

要修改几处地方:

设置扩展文件所在的目录：extension_dir="./"更改为extension_dir="C:/php7/ext/"

开启常用的扩展：

extension=php_gd2.dll

extension=php_mbstring.dll

extension=php_mysqli.dll

php的配置文件中;表示注释掉该行，所以记住要开启某个扩展，一定要去掉前面的;.

其他的配置无需理会，用到的时候再去学习如何配置。

接下来要配置Apache：

加载处理PHP的模块：在大概180行左右添加**loadModule php7_module "C:/php7/php7apache2_4.dll" ##加载php处理模块**

可以检查下这个文件是不是存在：

[![QQ截图20160129235635](/assets/images/2016/01/QQ截图20160129235635.png)](/assets/images/2016/01/QQ截图20160129235635.png)  在Apache中添加识别PHP的MIME类型：在AddType application/x-gzip .gz .tgz下一行添加

AddType application/x-httpd-php .php  ##php文件的MIME类型

AddType application/x-httpd-source .phps  ##phps文件的MIME类型

设置PHP配置文件目录:在最后一行添加

PHPIniDir "c:/php7"  ##指定php.ini目录

保存后，之前配置的php.ini复制到C:/WINDOWS/下，重启Apache服务。

我们来做一个简单的PHP页面访问测试：

在Apache的Web根目录下新建一个phptest.php的文件，输入以下内容：

&lt;?php

phpinfo();

?&gt;

打开浏览器，输入http://localhost/phptest.php,如果输出了php的版本信息，表示配置成功！

[![QQ截图20160130011055](/assets/images/2016/01/QQ截图20160130011055.png)](/assets/images/2016/01/QQ截图20160130011055.png)

开始愉快的PHP学习之旅吧！

注解1：VC14表示是由Visual C++ Redistribute 2015编译的，需要安装对应的版本，可以看到安装的时候后面显示的是14.0.23026，这个就是VC14。如果在启动服务的时候报错，安装VC14后就能解决。

[![QQ截图20160130003938](/assets/images/2016/01/QQ截图20160130003938.png)](/assets/images/2016/01/QQ截图20160130003938.png)

&nbsp;

&nbsp;