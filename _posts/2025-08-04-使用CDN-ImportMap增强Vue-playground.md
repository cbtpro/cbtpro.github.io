---
layout: post
title: "使用CDN、ImportMap增强Vue playground"
date: 2025-08-04
tags: ["前端", "每天一个知识点", "Vue.js", "源码", "GitHub", "React.js"]
---
有时候只是想试一小段代码，却不想开 VS Code、建项目、装依赖；想看看 TypeScript 怎么推断类型，也不想折腾太多。

这就是各种在线「演练场」（REPL）存在的意义。

无论是 Babel、TypeScript、Vue 还是 React Native，这些在线工具帮我们快速验证思路、测试用法、甚至调试模块导入问题，效率拉满。

下面介绍一下我经常使用的Vue SFC Playground的经验，主要是在封装vue、使用antdv组件时会使用到它。

## UNPKG

UNPKG 是一个基于 [npm](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2F) 的内容分发网络（CDN），可以让你通过 URL 快速加载 npm 包中的任意文件，适用于无需构建的前端项目或在线测试/演示。我们所有的依赖都从它这里请求。公司项目请不要使用，自己的学习项目可以使用。

## 基本用法

### 基本 URL 格式

```ruby
https://unpkg.com/:package@:version/:file
```

| 部分 | 说明 || --- | --- || :package | npm 包名 || :version | 可选，npm 版本号或 tag || :file | 包内的文件路径 |
### 示例：

```ruby
https://unpkg.com/preact@10.26.4/dist/preact.min.js
https://unpkg.com/react@18.3.1/umd/react.production.min.js
https://unpkg.com/three@0.174.0/build/three.module.min.js
```

### 使用 latest 或 semver 范围

```bash
https://unpkg.com/react@latest/umd/react.production.min.js
https://unpkg.com/preact@^10/dist/preact.min.js
```

>> 不写版本默认是 latest：>
```ruby
https://unpkg.com/vue/dist/vue.esm-browser.prod.js
```

## 浏览目录和元信息

>> ⚠️ 注意：目前某些目录 URL 已无法直接列出文件列表，但仍支持通过 ?meta 获取 JSON 目录结构。>
### 获取目录元信息：

```ruby
https://unpkg.com/react@18.3.1/?meta
https://unpkg.com/preact@10.26.4/src/?meta
```

返回示例：

```json
{
  "package": "react",
  "version": "18.3.1",
  "prefix": "/",
  "files": [
    {
      "path": "/umd/react.production.min.js",
      "type": "text/javascript",
      "size": 102393,
      "integrity": "sha256-..."
    }
  ]
}
```

## 文件路径解析规则

1. 若不指定文件，默认会使用 package.json 中的 main 字段。2. 支持 exports 字段，自动解析 default 和子路径。3. 支持通过 ?conditions 查询参数指定条件导出。
### 示例：

#### 默认导出：

```arduino
https://unpkg.com/jquery
```

等价于：

```bash
https://unpkg.com/jquery@<latest>/dist/jquery.js
```

#### exports.default 解析：

```json
"exports": {
  "default": "./dist/index.js"
}
```

则可使用：

```xml
<script src="https://unpkg.com/my-package"></script>
```

#### 子路径：

```json
"exports": {
  "./exp": {
    "default": "./dist/exp.js"
  }
}
```

```xml
<script src="https://unpkg.com/my-package/exp"></script>
```

#### 条件导出：

```ini
https://unpkg.com/react?conditions=react-server
```

## 自定义 UNPKG 入口文件

你可以在 `package.json` 中添加：

```json
"unpkg": "./dist/index.unpkg.js"
```

或：

```json
"exports": {
  "unpkg": "./dist/index.unpkg.js"
}
```

这样 `https://unpkg.com/my-package` 会使用你指定的文件。

## 浏览器访问

使用`https://app.unpkg.com/组件库名称`,则可以通过浏览器访问信息

```arduino
https://app.unpkg.com/axios
```

![image.png](/assets/images/d5a2d773e0e3.jpg)

可以直接访问package.json查看`export`和`import`信息

![image.png](/assets/images/05f7f27c762e.jpg)

因为并不是所有的库都支持es,只有支持es的模式,才可以通过importMap的方式来引入.

## 免构建项目示例（使用 Import Map）

```perl
<script type="importmap">
{
  "imports": {
    "preact": "https://unpkg.com/preact@10.25.4/dist/preact.module.js",
    "preact/hooks": "https://unpkg.com/preact@10.25.4/hooks/dist/hooks.module.js",
    "htm": "https://unpkg.com/htm@3.1.1/dist/htm.module.js"
  }
}

```

能够寻找到正确的cdn地址后，就可以配置到Vue SFC Playground的Import Map中，然后可以在组件内使用import的方式来使用

![image.png](/assets/images/051e32740889.jpg)

![image.png](/assets/images/6d73a8a77a48.jpg)

![image.png](/assets/images/a6a30b07564e.jpg)

可以亲手试一试，复制下面代码在浏览器中访问

```bash
https://play.vuejs.org/#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
```

>> 代码完成后,就可以将url分享给好友,但注意url是根据内容实时变更的,不会同步更新>
## 查找路径建议

1. 去 [npmjs.com](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2F) 搜索包名。2. 查看 package.json 信息3. 查看 dist/ 或 esm/ 目录下有哪些文件。4. 拼接完整的路径用于 UNPKG 访问。
## 更简单的方式： [ESM Run](https://link.juejin.cn?target=https%3A%2F%2Fesm.run%2F)

如果你的网络状况良好，ESM Run 是一个非常便捷的工具，它是一个用于调试 CDN 模块导入的 playground，支持测试以下方式的模块：

- npm: 协议- esm.sh、skypack.dev 等 CDN 模块源
你可以快速验证模块是否能通过 CDN 正确导入，适合调试 ES 模块和构建 import map 配置。

## 扩展

像这样的演练场有:

当然可以，以下是按你给的格式重新组织的常用前端在线演练场（REPL）工具：

像这样的演练场有：

- [Babel REPL](https://link.juejin.cn?target=https%3A%2F%2Fwww.babeljs.cn%2Frepl%23%3Fbrowsers%3Ddefaults%252C%2520not%2520ie%252011%252C%2520not%2520ie_mob%252011%26build%3D%26builtIns%3Dfalse%26corejs%3D3.21%26spec%3Dfalse%26loose%3Dfalse%26code_lz%3DMYewdgziA2CmB00QHMAUBGATAZgJRA%26debug%3Dfalse%26forceAllTransforms%3Dfalse%26modules%3Dfalse%26shippedProposals%3Dfalse%26evaluate%3Dfalse%26fileSize%3Dfalse%26timeTravel%3Dfalse%26sourceType%3Dmodule%26lineWrap%3Dtrue%26presets%3Denv%252Creact%252Cstage-2%26prettier%3Dfalse%26targets%3D%26version%3D7.28.2%26externalPlugins%3D%26assumptions%3D%257B%257D)

转换、调试 JavaScript 最新语法，查看编译结果（支持 React、TS 等插件）- [TypeScript Playground](https://link.juejin.cn?target=https%3A%2F%2Fwww.typescriptlang.org%2Fplay%2F%3F%23code%2FPTAEHUFMBsGMHsC2lQBd5oBYoCoE8AHSAZVgCcBLA1UABWgEM8BzM%2BAVwDsATAGiwoBnUENANQAd0gAjQRVSQAUCEmYKsTKGYUAbpGF4OY0BoadYKdJMoL%2BgzAzIoz3UNEiPOofEVKVqAHSKymAAmkYI7NCuqGqcANag8ABmIjQUXrFOKBJMggBcISGgoAC0oACCbvCwDKgU8JkY7p7ehCTkVDQS2E6gnPCxGcwmZqDSTgzxxWWVoASMFmgYkAAeRJTInN3ymj4d-jSCeNsMq-wuoPaOltigAKoASgAywhK7SbGQZIIz5VWCFzSeCrZagNYbChbHaxUDcCjJZLfSDbExIAgUdxkUBIursJzCFJtXydajBBCcQQ0MwAUVWDEQC0gADVHBQGNJ3KAALygABEgAN5QC%2BmoABFQAhHzFBTBPB3AFoPBmAAKWn0xnuVmUDnuACUwRUOGaHjIXkQ8D6HI4NC%2BbjMzHYDGYkH4sGg6kSlr0Ii8fLpDKZgj5SWxfPADlQAHJhAA5SASPlBFQAeS%2BZHegmdWkgR1QjgUrmkeFATjNOmGWH0KAQiGhwkuNok5uiIgMHGxCyYrA4PCCJSAA)
官方 TypeScript 调试工具，支持类型推导、错误提示、转译预览- [React Native Snack](https://link.juejin.cn?target=https%3A%2F%2Fsnack.expo.dev%2F)
在线运行 React Native 示例，支持模拟器预览（需登录）- [StackBlitz](https://link.juejin.cn?target=https%3A%2F%2Fstackblitz.com%2F)
类似 VSCode 的在线 IDE，支持完整 Node/npm、支持 Vue/React/Vite/Angular，全功能项目开发和 import map- [CodeSandbox](https://link.juejin.cn?target=https%3A%2F%2Fcodesandbox.io%2F)
强大的前端项目协作平台，支持 npm 安装、importmap、适合组件库演示、Vue/React 开发- [JSFiddle](https://link.juejin.cn?target=https%3A%2F%2Fjsfiddle.net%2F)
传统的 HTML/CSS/JS 编辑器，适合快速原型，不推荐现代模块化开发- [CodePen](https://link.juejin.cn?target=https%3A%2F%2Fcodepen.io%2F)
前端 UI 演示平台，支持动画、样式调试，适合 CSS/HTML 视觉展示项目- [Svelte REPL](https://link.juejin.cn?target=https%3A%2F%2Fsvelte.dev%2Frepl)
Svelte 官方在线编辑器，支持组件预览、动态参数，适合学习 Svelte
## 参考文档

[unpkg.com/](https://link.juejin.cn?target=https%3A%2F%2Funpkg.com%2F)
[developer.mozilla.org/zh-CN/docs/…](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FHTML%2FReference%2FElements%2Fscript%2Ftype%2Fimportmap)
