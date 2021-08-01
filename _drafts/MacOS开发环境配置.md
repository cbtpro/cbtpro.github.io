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

