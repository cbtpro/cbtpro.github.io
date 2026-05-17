---
layout: post
title: "React 状态管理：Easy-Peasy 入门指南"
date: 2026-02-26
tags: ["React.js", "每天一个知识点", "前端", "JavaScript", "前端框架", "面试"]
---
大家好！今天我们来聊聊 React 状态管理中的一个非常简单且高效的库——Easy-Peasy。它可以帮助我们轻松管理应用中的状态，让开发更加高效和清晰。

## 相关链接

[官网](https://link.juejin.cn?target=https%3A%2F%2Feasy-peasy.dev%2F)
[Github仓库](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fctrlplusb%2Feasy-peasy)
[npm](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Feasy-peasy)

#### 为什么选择 Easy-Peasy？

Easy-Peasy 是对 Redux 的一种抽象。它提供了一个重新构思的 API，专注于开发者体验，使你能够快速轻松地管理状态，同时利用 Redux 强大的架构保障，并与其广泛的生态系统进行集成。

所有功能均已内置，无需配置即可支持一个健壮且可扩展的状态管理解决方案，包括派生状态、API 调用、开发者工具等高级特性，并通过 TypeScript 提供完整的类型支持体验。

它社区活跃、更新稳定，跟随社区的 redux 版本同步更新。

- Zero configuration 零配置- No boilerplate 无冗余代码- React hooks based API 基于React Hook的API- Extensive TypeScript support 广泛的TypeScript支持- Encapsulate data fetching 封装数据获取- Computed properties 计算属性- Reactive actions 响应式动作- Redux middleware support 支持 Redux 中间件- State persistence 状态持久性- Redux Dev Tools 支持Redux开发工具- Global, context, or local stores 全局、上下文或本地存储- Built-in testing utils 内置测试工具- React Native supported 支持 React Native- Hot reloading supported 支持热重载
所有这些功能只需安装一个依赖项即可实现。

## 创建nextjs项目

```shell
(base) pe7er@pe7erdeMacBook-Pro github % npx create-next-app@latest
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/create-next-app 1151ms (cache updated)
Need to install the following packages:
create-next-app@14.2.13
Ok to proceed? (y)

npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/create-next-app 1071ms (cache miss)
npm http fetch POST 200 https://mirrors.cloud.tencent.com/npm/-/npm/v1/security/advisories/bulk 621ms
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/create-next-app/-/create-next-app-14.2.13.tgz 1034ms (cache miss)
✔ What is your project named? … nextjs-easy-peasy-template
✔ Would you like to use TypeScript? … No / Yes
✔ Would you like to use ESLint? … No / Yes
✔ Would you like to use Tailwind CSS? … No / Yes
✔ Would you like to use `src/` directory? … No / Yes
✔ Would you like to use App Router? (recommended) … No / Yes
✔ Would you like to customize the default import alias (@/*)? … No / Yes
✔ What import alias would you like configured? … @/*
Creating a new Next.js app in /Users/pe7er/Developer/github/nextjs-easy-peasy-template.
```

```ruby
(base) pe7er@pe7erdeMacBook-Pro github % npx create-next-app@latest
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/create-next-app 1151ms (cache updated)
Need to install the following packages:
create-next-app@14.2.13
Ok to proceed? (y)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/create-next-app 1071ms (cache miss)
npm http fetch POST 200 https://mirrors.cloud.tencent.com/npm/-/npm/v1/security/advisories/bulk 621ms
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/create-next-app/-/create-next-app-14.2.13.tgz 1034ms (cache miss)
✔ What is your project named? … nextjs-easy-peasy-template
✔ Would you like to use TypeScript? … No / Yes
✔ Would you like to use ESLint? … No / Yes
✔ Would you like to use Tailwind CSS? … No / Yes
✔ Would you like to use `src/` directory? … No / Yes
✔ Would you like to use App Router? (recommended) … No / Yes
✔ Would you like to customize the default import alias (@/*)? … No / Yes
✔ What import alias would you like configured? … @/*
Creating a new Next.js app in /Users/pe7er/Developer/github/nextjs-easy-peasy-template.
Using npm.
Initializing project with template: app-tw
Installing dependencies:
- react
- react-dom
- next
Installing devDependencies:
- typescript
- @types/node
- @types/react
- @types/react-dom
- postcss
- tailwindcss
- eslint
- eslint-config-next
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/react 720ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/react-dom 1250ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/next 2449ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@opentelemetry%2fapi 591ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@playwright%2ftest 1250ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/sass 422ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/typescript 2082ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@types%2fnode 1414ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@types%2freact 742ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@types%2freact-dom 566ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/postcss 309ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/tailwindcss 1018ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint 479ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-config-next 1234ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@eslint-community%2feslint-utils 365ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/undici-types 370ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@types%2fprop-types 487ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@eslint-community%2fregexpp 498ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/csstype 508ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@eslint%2fjs 606ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@eslint%2feslintrc 685ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@humanwhocodes%2fmodule-importer 326ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@nodelib%2ffs.walk 331ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/chalk 329ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ajv 629ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/doctrine 337ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@ungap%2fstructured-clone 663ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/debug 469ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@humanwhocodes%2fconfig-array 871ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/cross-spawn 555ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/escape-string-regexp 413ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/esutils 221ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-visitor-keys 351ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/espree 361ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-scope 376ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/file-entry-cache 251ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/find-up 316ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/esquery 626ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/fast-deep-equal 559ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/graphemer 291ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/glob-parent 399ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/globals 422ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ignore 384ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-glob 218ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-path-inside 223ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/imurmurhash 294ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/js-yaml 262ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/levn 163ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/json-stable-stringify-without-jsonify 337ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/lodash.merge 272ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/strip-ansi 279ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/optionator 301ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/natural-compare 414ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/minimatch 471ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@rushstack%2feslint-patch 296ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/text-table 470ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-import-resolver-node 446ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-import-resolver-typescript 567ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-plugin-import 519ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-plugin-jsx-a11y 598ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2feslint-plugin-next 958ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-plugin-react 393ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@swc%2fhelpers 391ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint%2fparser 1477ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/busboy 688ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/postcss 12ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/graceful-fs 350ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-plugin-react-hooks 1006ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/caniuse-lite 903ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint%2feslint-plugin 1840ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2fenv 1474ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/styled-jsx 723ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2fswc-darwin-arm64 831ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2fswc-linux-arm64-gnu 728ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2fswc-darwin-x64 894ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/loose-envify 302ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2fswc-linux-x64-gnu 1025ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/nanoid 365ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2fswc-win32-x64-msvc 851ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2fswc-win32-arm64-msvc 1187ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2fswc-linux-x64-musl 1279ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/source-map-js 269ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/arg 380ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@alloc%2fquick-lru 540ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/scheduler 1241ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/chokidar 492ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-glob 1ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/dlv 235ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/didyoumean 360ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/fast-glob 367ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/micromatch 324ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/lilconfig 401ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/jiti 638ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/normalize-path 479ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/object-hash 447ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2fswc-win32-ia32-msvc 2878ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next%2fswc-linux-arm64-musl 3484ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/postcss-load-config 302ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/picocolors 2077ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/postcss-js 509ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/picocolors 848ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/postcss-nested 206ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/postcss-import 803ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/postcss-selector-parser 398ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/resolve 409ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/sucrase 382ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint 5ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/jiti 1ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-visitor-keys 7ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/espree 8ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/minimatch 9ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/debug 11ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/fast-deep-equal 5ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/import-fresh 207ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/strip-json-comments 315ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/fastq 318ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/json-schema-traverse 352ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@nodelib%2ffs.scandir 397ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/uri-js 208ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/fast-json-stable-stringify 453ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@humanwhocodes%2fobject-schema 493ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ansi-styles 206ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/shebang-command 163ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/supports-color 268ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/path-key 365ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/which 340ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ms 363ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/esrecurse 375ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/acorn-jsx 352ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/estraverse 451ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/acorn 412ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/locate-path 183ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/flat-cache 393ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/path-exists 323ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/argparse 260ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-extglob 351ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/estraverse 657ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/brace-expansion 362ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/prelude-ls 548ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/prelude-ls 362ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/type-fest 661ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/word-wrap 229ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/type-check 550ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/deep-is 461ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ansi-regex 287ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/fast-levenshtein 360ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/type-check 365ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/parent-module 269ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/resolve-from 297ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/reusify 217ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/run-parallel 253ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@nodelib%2ffs.stat 363ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/queue-microtask 393ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/punycode 219ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/color-convert 261ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/has-flag 314ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/color-name 286ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/isexe 301ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/shebang-regex 355ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint 9ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-plugin-import-x 371ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/glob 498ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint%2ftype-utils 856ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ts-api-utils 958ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint%2ftypes 1264ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint%2futils 1615ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/resolve 13ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint%2fvisitor-keys 1670ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint%2fvisitor-keys 1291ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-core-module 518ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/fast-glob 1ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@nolyfill%2fis-core-module 533ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint%2fscope-manager 2208ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-module-utils 513ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/enhanced-resolve 687ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint%2fscope-manager 1509ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/get-tsconfig 609ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/array.prototype.findlastindex 180ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/doctrine 2ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-module-utils 2ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-bun-module 458ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-core-module 2ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/array.prototype.flatmap 249ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@rtsao%2fscc 451ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/hasown 231ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/array.prototype.flat 381ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/array-includes 674ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/object.values 372ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/object.fromentries 434ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/object.groupby 481ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/semver 371ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/tsconfig-paths 410ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/aria-query 241ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint%2ftypescript-estree 2575ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ast-types-flow 650ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/emoji-regex 509ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-iterator-helpers 487ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/damerau-levenshtein 591ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/axobject-query 623ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/safe-regex-test 344ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/estraverse 9ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/jsx-ast-utils 492ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/string.prototype.includes 446ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/axe-core 1117ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/language-tags 517ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/array.prototype.findlast 479ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/array.prototype.tosorted 564ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/object.entries 458ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/jsx-ast-utils 482ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/string.prototype.repeat 345ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/prop-types 545ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/string.prototype.matchall 679ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/foreground-child 164ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/path-scurry 299ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/minipass 343ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/jackspeak 447ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint 6ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/typescript 27ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/semver 2ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/micromatch 4ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/brace-expansion 5ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/glob-parent 6ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/merge2 320ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/supports-preserve-symlinks-flag 352ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/path-parse 617ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/graceful-fs 8ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/resolve-pkg-maps 513ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/tapable 631ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/call-bind 165ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/call-bind 186ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/define-properties 251ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-string 280ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/get-intrinsic 378ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-object-atoms 511ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/call-bind 5ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/define-properties 10ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-errors 288ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/define-properties 378ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-object-atoms 269ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-shim-unscopables 344ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-abstract 562ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-abstract 271ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-shim-unscopables 420ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-abstract 503ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-shim-unscopables 551ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/json5 287ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/function-bind 365ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@types%2fjson5 368ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/strip-bom 296ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/minimist 481ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-abstract 1950ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-define-property 193ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/set-function-length 251ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/has-property-descriptors 283ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/arraybuffer.prototype.slice 341ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/data-view-byte-length 133ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/define-data-property 493ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/data-view-buffer 272ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/object-keys 537ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/available-typed-arrays 358ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/array-buffer-byte-length 573ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/function.prototype.name 284ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-set-tostringtag 398ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/data-view-byte-offset 474ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/has-property-descriptors 11ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/gopd 250ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-to-primitive 397ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/globalthis 409ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/has-symbols 205ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/get-symbol-description 534ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/has-proto 298ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-data-view 185ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-callable 279ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-regex 201ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/internal-slot 460ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-array-buffer 521ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-negative-zero 332ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-shared-array-buffer 304ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-weakref 378ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/string.prototype.trim 210ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/object.assign 365ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/object-inspect 388ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-typed-array 547ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/regexp.prototype.flags 435ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/typed-array-byte-offset 138ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/typed-array-byte-length 171ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/safe-array-concat 550ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/has-proto 3ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/typed-array-length 204ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/unbox-primitive 168ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/string.prototype.trimstart 369ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/string.prototype.trimend 462ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/which-typed-array 189ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/typed-array-buffer 524ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/has-tostringtag 333ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/define-data-property 6ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-callable 7ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/has-tostringtag 9ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-shared-array-buffer 11ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/which-typed-array 4ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/possible-typed-array-names 128ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-symbol 179ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-date-object 258ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/isarray 271ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/functions-have-names 323ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/set-function-name 369ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/for-each 330ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/side-channel 517ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/for-each 384ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/for-each 544ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/which-boxed-primitive 359ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/has-bigints 517ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/for-each 528ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/language-subtag-registry 438ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/deep-equal 513ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/iterator.prototype 516ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/object-is 334ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-arguments 452ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/which-collection 508ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/es-get-iterator 577ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-set 282ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-set 5ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-number-object 376ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/stop-iteration-iterator 422ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-bigint 449ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-boolean-object 456ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-weakmap 189ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-map 566ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-map 604ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-weakset 434ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/reflect.getprototypeof 424ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/set-function-name 5ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/loose-envify 8ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/object-assign 156ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/react-is 1076ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/picomatch 242ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/braces 404ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/rimraf 287ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/keyv 422ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/flatted 709ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/p-locate 430ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/glob 8ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/json-buffer 406ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/signal-exit 220ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/lru-cache 234ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@pkgjs%2fparseargs 296ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@isaacs%2fcliui 367ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/callsites 198ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/which-builtin-type 496ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/strip-ansi 13ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/strip-ansi 13ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/wrap-ansi 188ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/wrap-ansi 261ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/string-width 387ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/string-width 518ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ansi-regex 23ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/string-width 22ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ansi-styles 25ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/emoji-regex 28ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-fullwidth-code-point 235ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eastasianwidth 381ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/p-limit 321ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/fill-range 488ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/to-regex-range 357ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-number 380ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/concat-map 382ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/balanced-match 385ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/react 17ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/source-map-js 10ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/nanoid 10ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/streamsearch 310ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@swc%2fcounter 441ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/tslib 499ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/client-only 1119ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/yocto-queue 300ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/js-tokens 222ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-generator-function 602ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-async-function 812ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-finalizationregistry 844ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/inherits 249ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/once 407ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/path-is-absolute 410ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/inflight 467ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/fs.realpath 522ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ts-node 1383ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@swc%2fcore 659ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@swc%2fhelpers 4ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@swc%2fwasm 707ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@types%2fnode 24ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/typescript 19ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/normalize-path 10ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/postcss-value-parser 243ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/lilconfig 5ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-binary-path 304ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/anymatch 308ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/postcss-selector-parser 6ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/read-cache 419ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/camelcase-css 422ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/cssesc 188ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/readdirp 554ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/util-deprecate 276ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/mz 166ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/fsevents 745ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/yaml 523ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/commander 377ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/lines-and-columns 322ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@jridgewell%2fgen-mapping 449ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/pirates 463ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/ts-interface-checker 656ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/binary-extensions 268ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/pify 159ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@jridgewell%2fsourcemap-codec 256ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/any-promise 312ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@jridgewell%2fset-array 315ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/thenify-all 467ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@jridgewell%2ftrace-mapping 684ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@jridgewell%2fresolve-uri 320ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/thenify 341ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/minimist 4ms (cache hit)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/wrappy 294ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/wrappy 345ms (cache updated)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next/env/-/env-14.2.13.tgz 411ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/get-tsconfig/-/get-tsconfig-4.8.1.tgz 412ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@nolyfill/is-core-module/-/is-core-module-1.0.39.tgz 428ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-config-next/-/eslint-config-next-14.2.13.tgz 462ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/eslint-import-resolver-typescript/-/eslint-import-resolver-typescript-3.6.3.tgz 520ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint/types/-/types-8.7.0.tgz 522ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@next/eslint-plugin-next/-/eslint-plugin-next-14.2.13.tgz 542ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint/parser/-/parser-8.7.0.tgz 564ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint/utils/-/utils-8.7.0.tgz 573ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint/visitor-keys/-/visitor-keys-8.7.0.tgz 581ms (cache miss)
npm http fetch POST 200 https://mirrors.cloud.tencent.com/npm/-/npm/v1/security/advisories/bulk 712ms
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint/typescript-estree/-/typescript-estree-8.7.0.tgz 676ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@types/prop-types/-/prop-types-15.7.13.tgz 675ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/is-bun-module/-/is-bun-module-1.2.1.tgz 679ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint/type-utils/-/type-utils-8.7.0.tgz 698ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@types/react/-/react-18.3.9.tgz 807ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint/eslint-plugin/-/eslint-plugin-8.7.0.tgz 918ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@typescript-eslint/scope-manager/-/scope-manager-8.7.0.tgz 927ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/@types/node/-/node-20.16.7.tgz 1032ms (cache miss)
npm http fetch GET 200 https://mirrors.cloud.tencent.com/npm/next/-/next-14.2.13.tgz 3489ms (cache miss)
added 360 packages, and audited 361 packages in 1m
137 packages are looking for funding
  run `npm fund` for details
found 0 vulnerabilities
Initialized a git repository.
Success! Created nextjs-easy-peasy-template at /Users/pe7er/Developer/github/nextjs-easy-peasy-template
```

项目创建完成后，应该启动一次项目，确保项目运行无误。

## 安装easy-peasy

首先，你需要安装easy-Peasy。可以使用 npm

```bash
npm install easy-peasy
```

#### 创建一个简单的状态管理模型

接下来，我们来创建一个简单的状态管理模型。假设我们要管理一个计数器的状态：

```javascript
import { createStore, action } from 'easy-peasy';

// 定义模型
const model = {
  count: 0,
  increment: action((state) => {
    state.count += 1;
  }),
  decrement: action((state) => {
    state.count -= 1;
  }),
};

// 创建 store
const store = createStore(model);
```

#### 在组件中使用 Easy-Peasy

接下来，我们可以在 React 组件中使用这个 store：

```javascript
import React from 'react';
import { StoreProvider, useStoreState, useStoreActions } from 'easy-peasy';

const Counter = () => {
  const count = useStoreState((state) => state.count);
  const { increment, decrement } = useStoreActions((actions) => ({
    increment: actions.increment,
    decrement: actions.decrement,
  }));

  return (

      {count}
      增加
      减少

  );
};

const App = () => {
  return (
    
      
    
  );
};

export default App;
```

#### 总结

通过以上步骤，我们实现了一个简单的计数器应用。Easy-Peasy 的使用极其简单，并且能够有效地管理组件间的状态。如果你正在寻找一个轻量级且易于使用的状态管理解决方案，不妨试试 Easy-Peasy！

希望这篇博客能帮助你更好地理解 Easy-Peasy 的使用。如果你有任何问题或想法，欢迎在评论区分享！
