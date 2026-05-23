---
layout: post
title: "nuxtjs+git submodule的微前端有没有搞头"
date: 2025-07-13
tags: ["前端", "前端框架", "设计模式", "每天一个知识点", "青训营笔记", "Nuxt.js"]
---
### 背景介绍

在中大型前端项目中，模块拆分和团队协作开发是常态。传统的微前端方案如 Qiankun、Module Federation 更偏向于 SPA 应用，难以兼容服务端渲染（SSR）场景。

Nuxt.js 本身支持模块化架构，可以将功能封装为独立的 Nuxt 模块（Module），具备以下特点：

- 支持 SSR，适合内容站、管理后台、运营平台等应用- 可复用性强，支持多个项目共享同一模块- 每个模块可独立开发、测试和发布，适合团队分工- 打包时主项目统一编译，避免子项目重复打包、加载
为了实现模块的独立性与可控性，本文结合 `git submodule` 和 Nuxt 模块机制，构建出一种基于 Git 仓库拆分、Nuxt 构建统一的“SSR 微前端”架构。每个模块作为一个 Git 子仓库单独维护，通过 `pnpm workspace` 管理依赖，在主项目中组合构建，实现模块解耦、分仓协作、统一部署的开发模式。

本文完整代码托管在

- [未使用git submodule](https://github.com/cbtpro/nuxtjs-modules-template)- [git submodule 主站](https://github.com/cbtpro/nuxtjs-base-template)- [git submodule 子站0](https://github.com/cbtpro/nuxtjs-module-0)- [git submodule 子站1](https://github.com/cbtpro/nuxtjs-module-1)
### 参考文档

[开始使用](https://nuxt.com.cn/docs/getting-started/installation)

[模块作者指南](https://nuxt.com.cn/docs/guide/going-further/modules/#quick-start)

[Modules](https://nuxt.com.cn/docs/api/kit/modules)

[生命周期钩子](https://nuxt.com.cn/docs/api/advanced/hooks)

### 创建主项目

```bash
npx create nuxt nuxtjs-base-template
```

使用空格选中、a键全选、上下方向键切换，选择你需要的模块，建议不要选中`@nuxt/content`、`@nuxt/fonts`、`@nuxt/icon`。我这里全选后敲回车继续。

![](/assets/images/7526309936814489646-1.jpg)

### 创建nuxt module，用于开发子项目

子项目和主项目的目录层级是平级的，每一个项目和模块都是单独的git仓库。

```text
- nuxtjs-base-template
- nuxtjs-module-0
- nuxtjs-module-1
```

创建module

```bash
pnpm create nuxt -t module nuxtjs-module-0
pnpm create nuxt -t module nuxtjs-module-1
```

创建时根据实际需求选择，这里建议不要选择`@nuxt/content`、`@nuxt/fonts`、`@nuxt/icon`。

![](/assets/images/7526309936814489646-2.jpg)

创建完成后，将主项目和子模块都提交到远端仓库

```shell
# 主项目
cd nuxtjs-base-template
git remote add origin git@github.com:cbtpro/nuxtjs-base-template.git
git branch -M main
git push -u origin main

# 子模块1
cd nuxtjs-module-0
git remote add origin git@github.com:cbtpro/nuxtjs-module-0.git
git branch -M main
git push -u origin main

# 子模块2
cd nuxtjs-module-1
git remote add origin git@github.com:cbtpro/nuxtjs-module-1.git
git branch -M main
git push -u origin main
```

接下来所有的操作都在主项目中操作

### 建立父子模块关系

```bash
cd nuxtjs-base-template
# 添加模块到packages目录下
git submodule add git@github.com:cbtpro/nuxtjs-module-0.git packages/nuxtjs-module-0
git submodule add git@github.com:cbtpro/nuxtjs-module-1.git packages/nuxtjs-module-1
```

最终形成的目录结构

![](/assets/images/7526309936814489646-3.jpg)

添加`pnpm-workspace.yaml`，并填入下面的内容。

```makefile
packages:
  - packages/*
```

说明

- packages是 pnpm 工作区（workspace） 的核心配置，用来告诉 pnpm 你的项目中有哪些子包（子模块），可以指定具体的目录，这里使用通配符
修改根目录的app.vue为以下内容

```xml

  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>

```

添加第一个页面`pages/index.vue`

```xml
<script setup lang="ts">

  <div>首页</div>

```

如果你在创建项目时开启了`@nuxt/content`，它需要安装`better-sqlite3`

```
pnpm install -D -w better-sqlite3
```

执行pnpm install时，控制台会报一个警告，这个警告必须处理，否则会影响后续的工程化执行。

![](/assets/images/7526309936814489646-4.jpg)

配置完`onlyBuiltDependencies`，需要删除主项目和子项目的依赖，重新再根目录使用`pnpm install`执行一遍安装依赖。

```bash
rm -rf node_modules pnpm-lock.yaml
rm -rf packages/nuxtjs-module-0/{node_modules,pnpm-lock.yaml}
rm -rf packages/nuxtjs-module-1/{node_modules,pnpm-lock.yaml}
pnpm i
```

### 

```bash
pnpm approve-builds
pnpm run dev
```

这个命令会列出当前 `pnpm install` 阻止执行构建脚本的依赖项，让你手动批准是否信任这些依赖，批准后，会在pnpm-workspace.yaml中生成onlyBuiltDependencies，如果全部批准过，则不会出现警告，也不会有选择的操作。

![](/assets/images/7526309936814489646-5.jpg)

![](/assets/images/7526309936814489646-6.jpg)

可以检查下预先执行了哪些脚本

![](/assets/images/7526309936814489646-7.jpg)

可以检查下`pnpm-workspace.yaml`，发现增加了配置`onlyBuiltDependencies`

![](/assets/images/7526309936814489646-8.awebp)

说明

- onlyBuiltDependencies 是解决默认安全行为，会忽略node_modules的内部脚本（例如：husky、axide、esbuild、vue-demi）
## 如果启用了@nuxt/content

启用了`@nuxt/content`，则需要在项目根目录添加`content.config.ts`

```php
/**
 * @see https://content.nuxt.com/docs/getting-started/configuration
 */
import { defineContentConfig, defineCollection } from '@nuxt/content';

export default defineContentConfig({
  collections: {
    content: defineCollection({
      type: 'page',
      source: '**/*.md'
    })
  }
});
```

添加`content\index.md`

```bash
# My First Page

Here is some content.
```

修改`pages\index.vue`

```xml
<script setup lang="ts">
const { data: home } = await useAsyncData(() => queryCollection('content').path('/').first());

useSeoMeta({
  title: home.value?.title,
  description: home.value?.description
});

  <ContentRenderer v-if="home" :value="home" />
  <div v-else>Home not found</div>

```

### 修改端口

可以修改下默认端口，方便固定开发地址。

```php
export default defineNuxtConfig({
  compatibilityDate: '2025-05-15',
  devServer: {
    host: '0.0.0.0',
    port: 8864,
    cors: {},
    https: false,
  },
  // 其他配置
}
```

### 启动项目

```bash
pnpm run dev
```

这时候主项目就启动了，如果你没有开启魔法上网，控制台会报谷歌字体文件加载失败。

![](/assets/images/7526309936814489646-9.awebp)

在nuxtjs.config.ts中禁用即可

```php
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  compatibilityDate: '2025-05-15',
  devServer: {
    host: '0.0.0.0',
    port: 8864,
    cors: {},
    https: false,
  },
  devtools: { enabled: true },
  // 禁用默认的 google 字体提供商
  fonts: {
    providers: {
      google: false,
      googleicons: false,
      bunny: false,
    }
  },
  modules: [
    '@nuxt/content',
    '@nuxt/eslint',
    // '@nuxt/fonts',
    '@nuxt/icon',
    '@nuxt/image',
    '@nuxt/scripts',
    '@nuxt/test-utils',
    '@nuxt/ui',
  ]
});
```

![](/assets/images/7526309936814489646-10.awebp)

此时主项目已经运行成功了，可以通过ip地址进行访问。

![](/assets/images/7526309936814489646-11.awebp)

### 添加开发脚本

检查下子模块的`packages\nuxtjs-module-0\package.json`中的`scripts`

```json
{
  "name": "my-module",
  "version": "1.0.0",
  "scripts": {
    "prepack": "nuxt-module-build build",
    "dev": "npm run dev:prepare && nuxi dev playground",
    "dev:build": "nuxi build playground",
    "dev:prepare": "nuxt-module-build build --stub && nuxt-module-build prepare && nuxi prepare playground",
    "release": "npm run lint && npm run test && npm run prepack && changelogen --release && npm publish && git push --follow-tags",
    "lint": "eslint .",
    "test": "vitest run",
    "test:watch": "vitest watch",
    "test:types": "vue-tsc --noEmit && cd playground && vue-tsc --noEmit"
  },
```

我们需要用到的命令有打包`prepack`、开发`dev`，我们需要将命令配置到父项目的文件`package.json`的`scripts`中。增加`dev:module`、`prepack:module`

```json
{
  "name": "nuxt-app",
  "private": true,
  "type": "module",
  "scripts": {
    "dev:module": "pnpm -r --filter ./packages/** run dev",
    "prepack:module": "pnpm -r --filter ./packages/** run prepack",
  },
}
```

#### 修改子模块的内容

子模块的目录结构如下，playground里的内容用于调试测试，编译打包时不会打进主项目中。而src目录是我们的子模块的开发目录，

![](/assets/images/7526309936814489646-12.awebp)

修改`packages\nuxtjs-module-0\src\runtime\plugin.ts`,增加具有标志的内容，方便控制台查看日志，` console.log('Plugin injected by my-module!')`可以修改成打印任何内容，这句话会在开发启动控制台看到。

```javascript
import { defineNuxtPlugin } from '#app'

export default defineNuxtPlugin((_nuxtApp) => {
  console.log('Plugin injected by my-module!')
})
```

添加页面和组件
页面和组件都添加到`packages\my-module-0\src\runtime`目录下

![](/assets/images/7526309936814489646-13.awebp)

在模块中的`packages\nuxtjs-module-0\src\module.ts`中添加路由并修改模块id

```typescript
import { resolve } from 'node:path';
import { defineNuxtModule, addPlugin, createResolver } from '@nuxt/kit';

// Module options TypeScript interface definition
export interface ModuleOptions { }

const routes = [
  {
    name: 'module-0-home',
    path: '/module-0-home',
    file: resolve(__dirname, './runtime/pages/index.vue')
  },
];
export default defineNuxtModule({
  meta: {
    name: 'my-module-0',
    configKey: 'myModule0',
  },
  // Default configuration options of the Nuxt module
  defaults: {},
  setup(_options, _nuxt) {
    const resolver = createResolver(import.meta.url);

    // Do not add the extension since the `.ts` will be transpiled to `.mjs` after `npm run prepack`
    addPlugin(resolver.resolve('./runtime/plugin'));
  },
  hooks: {
    'pages:extend'(pages) {
      pages.push(...routes);
    }
  }
});
```

![](/assets/images/7526309936814489646-14.awebp)
同理，其余的模块也按相同的办法修改。

在nuxtjs.config.ts中添加子模块的依赖

```php
// https://nuxt.com/docs/api/configuration/nuxt-config
import module0 from './packages/nuxtjs-module-0';
import module1 from './packages/nuxtjs-module-1';

export default defineNuxtConfig({
  compatibilityDate: '2025-05-15',
  devServer: {
    host: '0.0.0.0',
    port: 8864,
    cors: {},
    https: false,
  },
  devtools: { enabled: true },
  // 禁用默认的 google 字体提供商
  fonts: {
    providers: {
      google: false,
      googleicons: false,
      bunny: false,
    }
  },
  modules: [
    '@nuxt/content',
    '@nuxt/eslint',
    // '@nuxt/fonts',
    '@nuxt/icon',
    '@nuxt/image',
    '@nuxt/scripts',
    '@nuxt/test-utils',
    '@nuxt/ui',
    module0,
    module1
  ]
});
```

![](/assets/images/7526309936814489646-15.awebp)

此时一定会报错，是因为子模块没有编译`dist`，只需要执行编译或开发命令，就会自动编译dist，然后重启ts服务即可。

```shell
[21:20:35]  ERROR  Cannot find module 'D:\chenbitao\Developer\github\nuxtjs-base-template\packages\nuxtjs-module-0\dist\module.mjs'. Please verify that the package.json has a valid "main" entry
```

![](/assets/images/7526309936814489646-16.awebp)

![](/assets/images/7526309936814489646-17.awebp)

### 编译打包

常用命令

```bash
# 编译子模块
pnpm run prepack:module
# 开发主站
pnpm run dev
# 开发子模块
pnpm run dev:module
# 生成静态文件并预览
pnpm run generate && pnpm run preview

# 编译运行
pnpm run prepack:module && pnpm run build && node .output/server/index.mjs
```

在打包主项目的代码时，会自动将子模块的dist代码打入主站代码中，所以在进行主项目编译时，一定要先`pnpm run prepack:module`生成子模块的`dist`。

![](/assets/images/7526309936814489646-18.awebp)

### 提交代码

先提交子模块的代码，最后需要push根目录的项目代码，最终在仓库中显示是如下图，会将最后一次的commit id显示在文件夹后面。

![](/assets/images/7526309936814489646-19.awebp)

新同事开发也只需要`git clone --recurse-submodules repo-url`主站的代码，子模块的结果会自动保持，不需要再次使用`git submodule`来生成。
已经拉取的也可以使用`git submodule update --init --recursive`来补救

### 常见问题

- 下载失败，如下，大概率是网络问题，有多种方法解决，推荐使用switchHost来进行host管理。
```text
[nuxi 18:42:34]  ERROR  Error: Failed to download template from registry: Failed to download https://raw.githubusercontent.com/nuxt/starter/templates/templates/v3.json: TypeError: fetch failed
```

在host中添加下面的配置

```text
140.82.114.4 github.com
199.232.69.194 github.global.ssl.fastly.net
185.199.108.133 raw.githubusercontent.com
185.199.109.133 raw.githubusercontent.com
185.199.110.133 raw.githubusercontent.com
185.199.111.133 raw.githubusercontent.com
```

配置完成如下

![](/assets/images/7526309936814489646-20.awebp)

- Could not locate the bindings file.
```text
ERROR  Cannot start nuxt:  Could not locate the bindings file. Tried:       nuxi 20:35:39  
 → D:\chenbitao\Developer\github\node_modules\.pnpm\better-sqlite3@12.2.0\node_modules\better-sqlite3\build\better_sqlite3.node
```

better-sqlite3没有安装

- fonts.google.com加载失败
WARN  Could not fetch from [fonts.google.com/metadata/fo…](https://fonts.google.com/metadata/fonts). Will retry in 1000ms. 3 retries left.        20:37:41

[20:37:41]  WARN  Could not fetch from [fonts.google.com/metadata/ic…](https://fonts.google.com/metadata/icons?key=material_symbols&incomplete=true). Will retry in 1000ms. 3 retries left.

项目使用了`@nuxt/fonts`,创建项目是启用了`@nuxt/fonts`，后续可以在nuxtjs.config.ts中注释掉`@nuxt/fonts`
