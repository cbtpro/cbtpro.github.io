---
layout: post
title:  "使用wordpress和群发工具群发邮件"
date:   2016-05-05 21:52:23 +0800
categories: jekyll update
---

登录[http://echo.chenbitao.com/wp-login.php](http://echo.chenbitao.com/wp-login.php)

登录成功后点击上方菜单进入设置

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress1.png)

开始设置

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress2.png)

获取SMTP Password，登录你想用来发送邮件的QQ好的邮箱，点击设置，点击账号

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress3.png)

开启pop3/SMTP服务

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress4.png)

经过简单的短信校验后，获取到密码

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress5.png)

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress6.png)

将这个密码填入上面设置里的SMTP Password中。

设置完成后点击上方菜单进入发送

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress7.png)

也可以直接访问页面[http://echo.chenbitao.com/wp-admin/admin.php?page=send_test_email](http://echo.chenbitao.com/wp-admin/admin.php?page=send_test_email)

修改内容成你需要的文本。

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress8.png)

确认邮件收到后，开始抓取协议。

使用firefox进入到上面的发送页面。在页面上点击鼠标邮件，选择httpwatch。

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress9.png)

打开界面后，点击Record。然后点击Send Test Email按钮。记录完成后，点击Stop安妮停止录制。

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress10.png)

![](http://echo.chenbitao.com/wp-content/uploads/2016/05/050516_1352_wordpress11.png)上面就是获取的协议，找到绿色的ux_email_to=，把后面红色的部分替换成

&lt;$邮箱01地址$&gt;

在群发助手目录下 "大战协议----超级邮件群发系统\超级邮件群发系统"

新建一个"wordpress协议"文件夹,文件夹里新建一个文本文件，名字默认，内容就是上一步替换过的内容。

就可以通过这个协议来群发邮件了。