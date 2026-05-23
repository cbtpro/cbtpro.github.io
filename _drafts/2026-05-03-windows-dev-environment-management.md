---
layout: post
title: "window管理开发环境篇 - 持续更新"
date: 2026-05-03 01:50:38 +0800
categories: [开发工具]
tags: [Windows, Scoop, 开发环境, 前端, 后端]
---
我个人非常喜欢那种一键部署开发环境的方式，但时间一长，我们会淡忘如何部署开发环境，它会让我们失去对开发环境的控制。

下面我记录window环境下我是如何管理开发环境的。

## 安装Scoop

设置前提条件

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

典型安装

```powershell
irm get.scoop.sh | iex
```

使用代理安装

```powershell
iex "& {$(irm get.scoop.sh -Proxy 'http://<ip:port>')} -Proxy 'http://<ip:port>'"

# or
$env:HTTP_PROXY='http://<ip:port>'
$env:HTTPS_PROXY='http://<ip:port>'
irm get.scoop.sh | iex
```

但我更喜欢高级安装，可以指定软件安装目录

```powershell
irm get.scoop.sh -outfile 'install.ps1'

.\install.ps1 -ScoopDir 'D:\Applications\Scoop' -ScoopGlobalDir 'D:\GlobalScoopApps' -NoProxy
```

## Scoop管理java版本

Scoop 是一个 Windows 下的包管理工具，可以方便地安装、管理和切换多个 JDK 版本。以下是具体步骤：

安装 Scoop

在 PowerShell 中运行以下命令以安装 Scoop：

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
iwr -useb get.scoop.sh | iex
```

验证安装是否成功：

```shell
scoop help
```

1. 添加 Java Bucket
Scoop 默认不包含 Java 相关的包，需要添加 java bucket：

```powershell
scoop bucket add java
```

1. 搜索并安装 JDK
使用以下命令查看可用的 JDK 版本：

```powershell
scoop search jdk
```

安装所需的 JDK 版本，例如 OpenJDK 8 和 OpenJDK 17：

```
scoop install openjdk8-redhat

scoop install openjdk17
```

1. 切换 JDK 版本
使用 scoop reset 命令切换到指定的 JDK 版本。例如：

```powershell
scoop reset openjdk8-redhat
```

切换到 OpenJDK 17：

```powershell
scoop reset openjdk17
```

1. 验证当前 JDK 版本
在命令行中运行以下命令，确认当前使用的 JDK 版本：

```
java -version
```

注意事项

- Scoop 会自动配置 JAVA_HOME 环境变量，无需手动设置。- 如果需要全局切换 JDK，请确保系统变量中的 JAVA_HOME 指向 Scoop 的 current 文件夹路径。
通过以上步骤，您可以轻松管理和切换多个 JDK 版本，适应不同项目需求。
参考文档 [GitHub - ScoopInstaller/Install： 📥 Next-generation Scoop （un）installer](https://github.com/ScoopInstaller/Install)

附录
app列表 [Scoop - Apps](https://scoop.sh/#/apps)
bucket列表 [Scoop - Buckets](https://scoop.sh/#/buckets)

## scoop安装Maven

```shell
scoop search maven
scoop install maven
```

## scoop安装nodejs

```bash
scoop bucket add versions
scoop install versions/nodejs16
scoop reset versions/nodejs16
```

## scoop安装vscode

在[Scoop - Apps （vscode） --- Scoop - Apps (vscode)](https://scoop.sh/#/apps?q=vscode&p=1)搜索vscode，选择你要安装的版本来安装。

```bash
scoop bucket add extras
scoop install extras/vscode
```

由于 Scoop 安装软件的方式是“绿色便携版”（不主动修改你的系统注册表），所以 VS Code 默认不会出现在你的鼠标右键菜单里，也不会自动关联 `.py` 或 `.js` 等文件。

安装完成后，可以根据需求导入下面的注册表。

```javascript
Add Visual Studio Code as a context menu option by running:
reg import "D:\Applications\Scoop\apps\vscode\current\install-context.reg"
For file associations, run:
reg import "D:\Applications\Scoop\apps\vscode\current\install-associations.reg"
For github integration, run:
reg import "D:\Applications\Scoop\apps\vscode\current\install-github-integration.reg"
```

这段话是 Scoop 在安装完 VS Code 后给你的配置指南。

由于 Scoop 安装软件的方式是“绿色便携版”（不主动修改你的系统注册表），所以 VS Code 默认不会出现在你的鼠标右键菜单里，也不会自动关联 `.py` 或 `.js` 等文件。

如果你想让 VS Code 用起来和普通安装版一样方便，你需要手动运行这段代码提供的 `.reg`（注册表）文件。

### 更新vscode

同理更新只需要将install更换成update即可

```shell
scoop update extras/vscode
```

同理切换版本、降级则使用reset，后面使用@接版本号

```powershell
scoop reset extras/vscode@1.116.0
```

### 详细功能拆解

#### 1. 添加右键菜单 (Context Menu)

> reg import "D:...\install-context.reg"
- 作用：运行后，当你右键点击任何文件夹或文件时，菜单里会出现 “通过 Code 打开” (Open with Code) 的选项。- 推荐程度：⭐⭐⭐⭐⭐（必做，极大提升效率）。
#### 2. 关联文件格式 (File Associations)

> reg import "D:...\install-associations.reg"
- 作用：将各种代码文件（如 .txt, .json, .md 等）的默认打开方式设为 VS Code。- 注意：如果你已经习惯用其他编辑器（如 Notepad++）看特定文件，可以跳过这一步。- 推荐程度：⭐⭐⭐
#### 3. GitHub 集成 (GitHub Integration)

> reg import "D:...\install-github-integration.reg"
- 作用：注册 vscode:// 协议处理程序。这让你在浏览器（如 GitHub 网页）上点击“Open in Visual Studio Code”按钮时，能直接唤起本地的编辑器。- 推荐程度：⭐⭐⭐⭐
### 如何操作？

你不需要手动去找这些文件，只需要按照以下步骤操作：

1. 按下 Win + X，选择 终端（管理员） 或 PowerShell（管理员） 。2. 依次复制并粘贴你那段提示里的三行命令（以 reg import 开头的）。3. 每行按回车执行。如果弹出“是否确定要继续”的系统提示，点击是。
### ⚠️ 特别提醒

如果你以后通过 Scoop 卸载了 VS Code，记得去同一个目录下找到以 `uninstall-` 开头的 `.reg` 文件运行一下，否则你的右键菜单会残留无效的选项。

## Scoop安装IDEA

[Scoop - Apps (idea)](https://scoop.sh/#/apps?q=idea&p=1)

```shell
# 已经添加过了extras则不需要执行，直接直接安装命令
scoop bucket add extras
scoop install extras/idea
```

## 安装GoogleChrome

```bash
scoop install extras/googlechrome
```

## 安装codex

```
scoop install codex
```

## scoop安装nodejs

## Volta管理node版本

如果也可以使用Volta管理nodejs版本

参考文档 [Volta 快速安装配置和入门指南 | Volta](https://zh.voltajs.com/guide/getting-started.html)

## 常用软件

[Scoop - Apps ("https://github.com/ScoopInstaller/Extras" freedownloadmanager)](https://scoop.sh/#/apps?q=%22https%3A%2F%2Fgithub.com%2FScoopInstaller%2FExtras%22+freedownloadmanager&p=1)

[Scoop - Apps (Podman)](https://scoop.sh/#/apps?p=1&q=Podman)
[Scoop - Apps (podman-desktop)](https://scoop.sh/#/apps?q=podman-desktop&p=1)
