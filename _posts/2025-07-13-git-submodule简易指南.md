---
layout: post
title: "git submodule简易指南"
date: 2025-07-13
tags: ["Git", "每天一个知识点", "前端", "Android", "VIM", "后端"]
---
## 场景说明

- 你想在一个主仓库中引用另一个 Git 仓库（如组件库、工具库）；- 保持两个仓库的独立性，但又能在主项目中集成使用；- 子模块中的代码不会直接包含在主仓库的 Git 记录中，只记录引用的 commit。
## 子模块初始化

### 克隆项目时自动拉取子模块：

```bash
git clone --recurse-submodules <repo-url>
```

如果已经克隆了项目，但未拉取子模块，请执行：

```css
git submodule update --init --recursive
```

## 子模块目录结构（示例）

```go
my-project/
├── .gitmodules
├── packages/
│   └── shared-ui/         ← 子模块（实际是另一个 Git 仓库）
│       ├── components/
│       └── package.json
```

## 添加子模块

```xml
git submodule add <git-url> <target-folder>
```

示例：

```bash
git submodule add https://github.com/your-org/shared-ui.git packages/shared-ui
```

提交变更：

```bash
git commit -am "chore: add submodule shared-ui"
```

## 更新子模块

如果子模块有更新：

```bash
cd packages/shared-ui
git pull origin main
```

然后在主项目中提交引用的 commit 更新：

```bash
cd ../..
git add packages/shared-ui
git commit -m "chore: update shared-ui submodule"
```

## 移除子模块

1. 删除记录：
```bash
git submodule deinit -f packages/shared-ui
git rm -f packages/shared-ui
rm -rf .git/modules/packages/shared-ui
```

1. 清理 .gitmodules 中的条目：
```bash
# 编辑 .gitmodules，手动删除对应子模块条目
```

## 注意事项

| 注意点 | 描述 || --- | --- || 子模块是只读 | 默认你不能在主项目中直接修改子模块内容，需要切换到子模块目录进行修改。 || 提交更新需两次 commit | 修改子模块后，需要提交子模块自身的变更，然后回到主仓库提交 commit 引用更新。 || 勿忘 .gitmodules | 这个文件记录子模块来源及路径，需一起提交。 || CI/CD 中记得拉取子模块 | 如：git submodule update --init --recursive |
## 参考资料

- [Git Submodule 官方文档](https://git-scm.com/book/en/v2/Git-Tools-Submodules)- [Pro Git 书籍：Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
