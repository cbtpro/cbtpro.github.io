# MacOS使用系统自带apache服务托管文件

MacOS自带apache，可以直接使用托管文件。

## 使用apache

```shell
# 启动 apache 
sudo apachectl start
# 重启 apache
sudo apachectl restart
# 停止 apache
sudo apachectl stop
# 测试
curl 127.0.0.1
# 输出<html><body><h1>It works!</h1></body></html>
```

## apache容器目录

apache默认根目录在`/Library/WebServer/Documents`，打开Finder，使用命令`Shift`+`Commond`+`G`打开前往文件夹，并填入apache根目录路径点击前往即可以使用Finder打开使用鼠标操作文件。

也可以直接通过`cp`或`mv`命令进行文件操作。



## 应用

可以将pac文件托管在apache中，设置系统代理。