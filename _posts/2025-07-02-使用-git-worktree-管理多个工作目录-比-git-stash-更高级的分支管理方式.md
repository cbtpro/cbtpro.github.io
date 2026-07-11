---
layout: post
title: "使用 `git worktree` 管理多个工作目录，比 `git stash` 更高级的分支管理方式"
date: 2025-07-02
tags: []
---
## 简介

`git worktree` 是 Git 提供的一个子命令，允许在一个仓库中创建多个额外的工作目录（工作树），每个目录可关联一个独立的分支，从而实现多个分支的并行开发，无需频繁切换分支或使用 `git stash` 暂存。

## 常见用途

| 用途 | 命令 || --- | --- || 添加一个工作目录用于新分支 | git worktree add ../feature-dir feature-branch || 列出当前所有 worktree | git worktree list || 删除 worktree（不会删除分支） | git worktree remove ../feature-dir |
## 示例：并行开发多个分支

```bash
# 进入项目目录
cd my-project

# 为 feature/login 创建一个工作目录
git worktree add ../my-project-login feature/login

# 为 bugfix/crash 创建另一个工作目录
git worktree add ../my-project-crash bugfix/crash
```

此后你可在使用命令行`cd`在两个目录中分别独立开发、提交，互不干扰。当分支`feature_1.0.0`被创建`worktree`后，在另一个`worktree`目录上是不能切换成`feature_1.0.0`在两个目录中分别独立开发、提交，互不干扰。

## 与 git stash 对比

| 功能/特性 | git stash | git worktree || --- | --- | --- || 作用 | 暂存当前更改，快速切分支 | 管理多个工作目录并行开发多个分支 || 多分支并行开发支持 | 否，仅能切换一个分支 | 是，每个工作目录一个分支 || 文件状态保存方式 | 储存在 stash 栈中 | 实际存在于多个工作目录 || 推荐使用场景 | 快速临时切换分支，不希望提交当前更改 | 长期并行开发多个特性/修复任务 |
## 与复制项目文件夹的对比

| 功能/特性 | git worktree | 复制文件夹（cp -r） || --- | --- | --- || Git 支持 | 原生支持，多个工作目录共享 .git 数据 | 每个目录都是独立 Git 仓库副本，彼此无关 || 空间占用 | 小，.git 数据不重复，源码为主 | 大，.git 和代码文件全部复制 || 分支独立性 | 高，每个目录可以检出独立分支 | 分支也可独立，但管理多个仓库代价更高 || 恢复安全性 | 强，误删工作目录后可从主仓库恢复 | 弱，误删后无直接恢复方式（需备份） || 推荐用途 | 推荐用于 Git 管理内的并行协作开发 | 推荐用于导出/迁移到其他非 Git 管理环境或测试 |
## 存储空间大小说明

使用 `git worktree` 创建的目录非常轻量，结构如下：

- 新目录中 .git 实际是一个指向主仓库 .git/worktrees/xxx 的文件；- 所有 Git 对象（如提交历史、快照）依旧存储在主仓库中；- 每个工作目录仅包含源码文件，没有重复的 Git 对象副本。
### 查看大小示例：

```bash
# 查看主仓库的 Git 数据大小
du -sh .git

# 查看 worktree 的目录大小（一般仅为代码文件大小）
du -sh ../my-project-login
```

对比复制项目文件夹，`git worktree` 占用空间更小、效率更高。

## 误删 worktree 目录后的恢复方法

如果误删了某个 worktree 目录，例如：

```bash
rm -rf ../my-project-login
```

执行 `git worktree list` 会看到如下异常：

```bash
/path/to/my-project        [main]
../my-project-login        (error)
```

### 恢复流程：

1. 清理 Git 内部的无效记录

```bash
git worktree prune
```

这将移除 .git/worktrees/ 下失效的引用，清理异常状态。2. 重新添加工作目录
若对应分支仍存在：

```bash
git worktree add ../my-project-login feature/login
```

即可重新创建并恢复该目录。
## 注意

使用 worktree 的分支目录没有.git目录，有些commit-msg 没有考虑worktree，脚本会获取项目根目录下的.git目录，没有考虑worktree的场景。

## 使用建议

> 删除工作目录前，请始终使用 git worktree remove ，确保主仓库中不残留失效引用，避免后续 Git 报错。
