---
layout: post
title: "使用 force-resolutions 强制锁定 npm/yarn 依赖版本"
date: 2025-06-23
tags: ["前端", "JavaScript", "每天一个知识点", "Vue.js", "Node.js", "NPM"]
---
在前端项目中，我们常常遇到间接依赖版本冲突、漏洞依赖未及时修复、预发版本不稳定等问题。当这些问题出现在依赖树深层，而你无法直接控制依赖版本时，`force-resolutions` 是一种非常实用的解决方案。

## 什么是 force-resolutions？

`force-resolutions` 是一个工具，此软件包修改 package-lock.json 以强制安装一组传递依赖项（依赖项的依赖项）的指定版本。

它的作用：

- 修复第三方包引入了错误版本的依赖- 强制降级/升级不兼容的依赖版本- 快速规避临时问题（如 beta 版、破坏性更新）
## 工作原理简述

- 你在 package.json 中配置 "resolutions" 字段- 安装 force-resolutions，在 preinstall 阶段运行- 它会修改 package-lock.json 或 yarn.lock 中的依赖版本，使其符合你设定的 resolution
## 适用场景举例

1. 某个依赖引用了漏洞版本的 lodash，但你不能直接升级它2. 某个 npm 包引用了错误的 beta 包（例如 @rolldown/pluginutils@beta-commit.56abf23）3. 多个插件依赖 tslib 但版本不一致导致重复打包
## 安装与配置

### 安装依赖

```css
npm install force-resolutions --save-dev
```

### 配置 package.json

添加 `resolutions` 字段来声明你想强制替换的依赖版本：

```json
{
  "resolutions": {
    "@rolldown/pluginutils": "1.0.0-beta.8"
  },
  "scripts": {
    "preinstall": "npx force-resolutions"
  }
}
```

> ⚠️ 注意：force-resolutions 必须放在 preinstall，否则 lock 文件不会被改写。
## 示例：统一 tslib 版本为 2.6.2

```json
{
  "resolutions": {
    "tslib": "2.6.2"
  },
  "scripts": {
    "preinstall": "npx force-resolutions"
  }
}
```

当你 `npm install` 时，`force-resolutions` 会：

- 修改 package-lock.json 中所有 tslib 版本为 2.6.2- 然后再进行实际安装
## yarn用户更简单！

如果你使用的是 Yarn（v1）：

```json
{
  "resolutions": {
    "tslib": "2.6.2"
  }
}
```

直接运行 `yarn install` 即可，无需额外安装 `force-resolutions`，这是 Yarn 原生支持的功能。

## 注意事项

1. force-resolutions 是 基于 lock 文件操作的，修改后应提交 package-lock.json2. 对于 pnpm 用户，该工具不适用（pnpm 有自己独特的锁定机制）3. resolutions 不支持通配符，必须明确指定完整依赖名4. 每次运行 npm install 前，preinstall 会自动确保 resolution 生效
## 为什么不直接用 overrides？

`npm` 在 v8.3+ 引入了 [`overrides`](https://docs.npmjs.com/cli/v8/configuring-npm/package-json#overrides) 功能，用法类似：

```text
"overrides": {
  "@rolldown/pluginutils": "1.0.0-beta.8"
}
```

但：

- overrides 只在 npm >= 8.3 有效- 它是 纯 npm 机制，不兼容 Yarn、pnpm- force-resolutions 更通用，兼容老项目（特别是 Yarn 项目）
## pnpm 用户

对于 pnpm 用户，使用 `force-resolutions` 是不兼容的 —— 因为 pnpm 的依赖管理机制和 npm/yarn 有显著不同，它不使用 `node_modules` 扁平结构，也不修改 `package-lock.json`，所以：

> ❌ pnpm 无法使用 force-resolutions。
但 pnpm 本身提供了 官方推荐的替代方案，叫做：

## pnpm.overrides —— 官方方式强制依赖版本

从 `pnpm v7+` 开始，你可以在 `package.json` 中添加：

```json
{
  "pnpm": {
    "overrides": {
      "@rolldown/pluginutils": "1.0.0-beta.8"
    }
  }
}
```

然后运行：

```
pnpm install
```

`pnpm v10`则使用`pnpm-workspace.yaml`配置

```bash
overrides:  
"foo": "^1.0.0"  
"quux": "npm:@myorg/quux@^1.0.0"  
"bar@^2.1.0": "3.0.0"  
"qar@1>zoo": "2"
```

pnpm 会：

- 强制使用你指定的版本- 在 .pnpm-lock.yaml 中锁定它- 对所有依赖树生效
## 示例：修复 @rolldown/pluginutils 问题

```json
{
  "pnpm": {
    "overrides": {
      "@rolldown/pluginutils": "1.0.0-beta.8"
    }
  }
}
```

如果你还需要修复多级依赖中的 tslib 等版本冲突，也可以这样：

```json
{
  "pnpm": {
    "overrides": {
      "tslib": "2.6.2",
      "some-lib > react": "^18.2.0"
    }
  }
}
```

✅ 支持嵌套（`A > B`）格式，可以精确覆盖特定依赖路径下的版本。

## 🧩 附加建议

1. 使用 .npmrc 锁定 pnpm 版本（例如 monorepo 工程）：

ini
```bash
package-manager=pdm@8.15.4
2. CI 中添加 pnpm install --frozen-lockfile 保障依赖版本一致性3. 不要手动修改 .pnpm-lock.yaml，统一通过 overrides 管理冲突版本
## 总结

| 特性 | force-resolutions | yarn resolutions | npm overrides || --- | --- | --- | --- || 是否适用于 npm | ✅ | ❌ | ✅（>=8.3） || 是否适用于 yarn | ✅ | ✅（原生） | ❌ || 修改 lock 文件 | ✅ | ✅ | ✅ || 支持多项目 monorepo | ✅ | ✅ | ✅ || 支持深度 patch | ✅ | ✅ | ✅ |
## 参考资料
```

- [force-resolutions GitHub](https://github.com/rogeriochaves/force-resolutions)- [Yarn Resolutions 文档](https://classic.yarnpkg.com/en/docs/selective-version-resolutions/)- [npm overrides 官方文档](https://docs.npmjs.com/cli/v8/configuring-npm/package-json#overrides)
## 最佳实践建议

- 优先使用 yarn 原生 resolutions（如在 monorepo 项目）- 如果是 npm 且版本 force-resolutions 配合 preinstall- 项目 CI 中应校验 lock 是否被污染，避免 resolution 被破坏- 加上 .nvmrc/engines 锁定 node/npm/yarn 版本，确保一致性
## 实战

下面以开发中遇到的一个例子来展示。
我们需要修改`package.json`,增加scripts和resolutins配置，格式如下。

```json
{
    "scripts": {
        "preinstall": "npx force-resolutions"
    },
    "resolutions": {
        "@xxx/xxx": "1.0.0"
    }
}
```

我们在使用jsx时，运行项目是报错。报`Error [ERR_MODULE_NOT_FOUND]: Cannot find module '/Users/chenbitao/Developer/github/antdv-vue-admin/node_modules/@rolldown/pluginutils/dist/esm/index.js' imported from /Users/chenbitao/Developer/github/antdv-vue-admin/node_modules/@vitejs/plugin-vue-jsx/dist/index.mjs`

```shell
% npm run dev
npm info using npm@10.8.2
npm info using node@v20.18.0

> vue3-antdv-project@0.0.0 dev
> vite

failed to load config from /Users/chenbitao/Developer/github/antdv-vue-admin/vite.config.ts
error when starting dev server:
Error [ERR_MODULE_NOT_FOUND]: Cannot find module '/Users/chenbitao/Developer/github/antdv-vue-admin/node_modules/@rolldown/pluginutils/dist/esm/index.js' imported from /Users/chenbitao/Developer/github/antdv-vue-admin/node_modules/@vitejs/plugin-vue-jsx/dist/index.mjs
    at finalizeResolution (node:internal/modules/esm/resolve:265:11)
    at moduleResolve (node:internal/modules/esm/resolve:933:10)
    at defaultResolve (node:internal/modules/esm/resolve:1169:11)
    at ModuleLoader.defaultResolve (node:internal/modules/esm/loader:542:12)
    at ModuleLoader.resolve (node:internal/modules/esm/loader:510:25)
    at ModuleLoader.getModuleJob (node:internal/modules/esm/loader:239:38)
    at ModuleWrap.<anonymous> (node:internal/modules/esm/module_job:96:40)
    at link (node:internal/modules/esm/module_job:95:36)
```

![](/assets/images/7518275819531370523-1.jpg)

通过`search node_modules`插件发现依赖`@rolldown/pluginutils`下面没有js文件。

![](/assets/images/7518275819531370523-2.jpg)
[www.npmjs.com/package/@ro…](https://www.npmjs.com/package/@rolldown/pluginutils/v/1.0.0-beta.8-commit.56abf23?activeTab=code)

再访问npm上对应的的版本，发现在一天前npm有被publish一次，但是列表可以看到一个月前发布的。

![](/assets/images/7518275819531370523-3.jpg)

列表没有取最新的时间，但详情页表示确实推送过，就是这次推送，导致了历史的版本内容消失，本地重新安装依赖后就报错。

![](/assets/images/7518275819531370523-4.jpg)

而最新的版本，也是昨天推送的，可以标记为弃用，但还是选择了直接重新推一个空的仓库，可能是作者故意为之，想将仓库里有重大问题的仓库代码直接删除。

[www.npmjs.com/package/@ro…](https://www.npmjs.com/package/@rolldown/pluginutils/v/1.0.0-beta.18?activeTab=code)

![](/assets/images/7518275819531370523-5.jpg)

找到问题再解决就很容易了，因为我使用的是`npm`，所以我选择使用`force-resolutions`。

这里的包名要写对，版本号规则和`dependencies`一致，支持指定版本范围版本。

```json
{
  // 其他配置
  "scripts": {
    "preinstall": "npx force-resolutions"
  },
  "resolutions": {
    "@rolldown/pluginutils": "1.0.0-beta.18"
  }
  // 其他配置
}
```

```
npm install
```

输出以下内容就表示成功，可以再回去去检查`node_modules`和`package-lock.json`中是否有修改成功。

```shell
% npm install
npm info using npm@10.8.2
npm info using node@v20.18.0
npm http fetch POST 200 https://mirrors.tencent.com/npm/-/npm/v1/security/advisories/bulk 630ms

> vue3-antdv-project@0.0.0 preinstall
> npx force-resolutions

npm info using npm@10.8.2
npm info using node@v20.18.0
npm http fetch GET 200 https://mirrors.tencent.com/npm/force-resolutions 264ms (cache revalidated)
Applying forced resolutions:
@rolldown/pluginutils => 1.0.0-beta.18
Finished applying forced resolutions.
npm info ok

up to date, audited 566 packages in 3s

160 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm info ok
```

![](/assets/images/7518275819531370523-6.jpg)

再次运行项目，问题解决。
