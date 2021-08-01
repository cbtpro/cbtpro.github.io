# MacOS开发环境配置

## 安装git

## 公钥管理

参考https://gitee.com/help/categories/38

### 配置中文环境

参考https://git-scm.com/book/zh/v2

解决git status中文乱码

```shell
git config --global core.quotepath false
```

解决git log中文乱码

```shell
git config --global i18n.commitencoding utf-8
git config --global i18n.logoutputencoding utf-8
export LESSCHARSET=utf-8
```

设置git pull的默认合并行为

```shell
git config pull.rebase true
```

## 安装Docker

Intel芯片直接从官网首页下载，Apple M1芯片从https://docs.docker.com/docker-for-mac/apple-silicon/下载安装，安装完成后即可正常使用。

## 安装VSCode

## 安装微信小程序开发环境

## 安装支付宝小程序开发环境

## 安装taro开发环境

https://taro.aotu.io/

## 安装chameleon开发环境

https://cml.js.org/

## 安装uni-app开发环境

https://uniapp.dcloud.io/
