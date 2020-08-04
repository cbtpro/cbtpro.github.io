mac安装ubuntu搭建cvat开发环境

## 安装docker



## 使用docker安装ubuntu

```shell
# 搜索ubuntu
docker search ubuntu
# 拉取ubuntu
docker pull ubuntu
# 查看安装的镜像
docker images

docker run --privileged -it -d --name cvat-dev-env -p 10022:22 -p 7000:7000 -p 3000:3000 ubuntu
412da43e193e3855f4212c3eff556d36845b717f4e02e96f31ad6808e7b61f36

docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
412da43e193e        ubuntu              "/bin/bash"         30 seconds ago      Up 28 seconds       0.0.0.0:8088->80/tcp   cvat-dev-env

# 查看容器信息，41是容器id开头的数字或者容器名称也可以
docker inspect 41 # or docker inspect cvat-dev-env
# 进入ubuntu开发环境
docker exec -it cvat-dev-env /bin/bash
```

更新

```shell
apt-get update
```

安装git

```shell
apt-get install git wget vim
```



开始部署开发环境https://github.com/opencv/cvat/blob/master/CONTRIBUTING.md

安装依赖，注意这里就不需要安装python相关的东西了

```shell
apt-get --no-install-recommends install -y ffmpeg build-essential nodejs npm curl redis-server libldap2-dev libsasl2-dev
```

安装过程中需要选择地区市区，选择亚洲和上海即可。

安装freetype

```shell
wget https://nchc.dl.sourceforge.net/project/freetype/freetype2/2.10.1/freetype-2.10.1.tar.gz
tar -zxvf ./freetype-2.10.1.tar.gz
cd freetype-2.10.1
./configure
make
make install
```

安装docker中linuxkit缺少的依赖

```shell
apt-get install libgeos-dev pkg-config libpng-dev
```

安装sqlite3

```shell
wget https://www.sqlite.org/2020/sqlite-autoconf-3310100.tar.gz
tar -zxvf ./sqlite-autoconf-3310100.tar.gz
cd sqlite-autoconf-3310100
./configure
make
make install
```

安装python3.6，cvat中要求的tensorflow1.15.2只支持到了python3.6

```shell
wget https://www.python.org/ftp/python/3.6.10/Python-3.6.10.tar.xz
tar -xvf ./Python-3.6.10.tar.xz
cd Python-3.6.10
./configure
make
make install
```



## 更改pip镜像地址

```shell
# 创建文件~/.pip/pip.conf，如果目录~/.pip不存在，就先创建
vim ~/.pip/pip.conf
# 将下面内容写进去
[global]
trusted-host =  mirrors.aliyun.com
index-url = https://mirrors.aliyun.com/pypi/simple
```

完成上面的几步后，为了让环境变量生效，需要退出ubuntu重新进入ubuntu

克隆cvat

```shell
mkdir -p ~/Developer/gitrepo
cd ~/Developer/gitrepo
git clone https://github.com/opencv/cvat
```



安装opens-server

```shell
apt-get install openssh-server
# 重启openssh-server
service ssh --full-restart
# 自动启动
systemctl enable ssh
# 备份配置文件
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
# 修改配置文件
vi /etc/ssh/sshd_config
# 删除Port前的#
Port 22
PermitRootLogin yes
PasswordAuthentication yes
# 保存后重启服务
service ssh --full-restart
```



打开本地vscode安装Remote Development插件

打开vscode设置，开启Show Login Terminal

使用Ctrl + p，输入> Remote-ssh: OpenConfiguration File...，来配置ssh地址配置





如果网络不佳，可以先克隆gitee上的仓库，然后再将仓库地址更改回github的仓库，在公司请使用公司自己的代码仓库

```shell
# 本仓库于2020年5月2日02:20分clone的官方仓库代码
git clone https://gitee.com/cbtpro/cvat.git
cd cvat
git pull
git remote rm origin
git remote add origin https://github.com/opencv/cvat.git
git pull
git branch --set-upstream-to=origin/master master
```

