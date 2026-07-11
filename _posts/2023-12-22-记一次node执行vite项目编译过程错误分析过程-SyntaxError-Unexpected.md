---
layout: post
title: "记一次node执行vite项目编译过程错误分析过程 SyntaxError: Unexpected token '??='"
date: 2023-12-22
tags: ["前端", "Vue.js", "Vite", "Webpack", "WebView", "后端"]
---
这是一次很常见的node开发环境搭建过程中的报错，也可能是在尝试运行别人的程序、开源项目碰到，很简单的问题，这篇文章详细介绍了我是如何去定位和解决问题的。

## 控制台输出

![](/assets/images/7315240062651449385-1.jpg)

```shell
bitao.chen@wzqzd2307070850 MINGW64 ~/Documents/Developer/github/tag-expand-demo (main)
$ npm run build:test

> vite-vue3-ts-template@0.0.0 build:test C:\Users\bitao.chen\Documents\Developer\github\tag-expand-demo
> cross-env NODE_ENV=development vue-tsc && vite build

C:\Users\bitao.chen\Documents\Developer\github\tag-expand-demo\node_modules\@vue\language-core\out\generators\template.js:224
                        tagOffsetsMap[tag] ??= [];
                                           ^^^

SyntaxError: Unexpected token '??='
    at wrapSafe (internal/modules/cjs/loader.js:1029:16)
    at Module._compile (internal/modules/cjs/loader.js:1078:27)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1143:10)
    at Module.load (internal/modules/cjs/loader.js:979:32)
    at Function.Module._load (internal/modules/cjs/loader.js:819:12)
    at Module.require (internal/modules/cjs/loader.js:1003:19)
    at require (internal/modules/cjs/helpers.js:107:18)
    at Object. (C:\Users\bitao.chen\Documents\Developer\github\tag-expand-demo\node_modules\@vue\language-core\out\index.js:18:14)
    at Module._compile (internal/modules/cjs/loader.js:1114:14)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1143:10)
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! vite-vue3-ts-template@0.0.0 build:test: `cross-env NODE_ENV=development vue-tsc && vite build`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the vite-vue3-ts-template@0.0.0 build:test script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\bitao.chen\AppData\Roaming\npm-cache\_logs\2023-12-22T11_50_30_592Z-debug.log
```

## 分析过程

从输出中可以看到最后两句有完整的日志信息所在的文件目录。

> 完整的日志信息<code>npm ERR! A complete log of this run can be found in: npm ERR!     C:\Users\bitao.chen\AppData\Roaming\npm-cache\_logs\2023-12-22T09_55_47_672Z-debug.log</code></p
```shell
0 info it worked if it ends with ok
1 verbose cli [
1 verbose cli   'C:\\Program Files\\nodejs\\node.exe',
1 verbose cli   'C:\\Program Files\\nodejs\\node_modules\\npm\\bin\\npm-cli.js',
1 verbose cli   'run',
1 verbose cli   'build:test'
1 verbose cli ]
2 info using npm@6.14.18
3 info using node@v14.21.3
4 verbose run-script [ 'prebuild:test', 'build:test', 'postbuild:test' ]
5 info lifecycle vite-vue3-ts-template@0.0.0~prebuild:test: vite-vue3-ts-template@0.0.0
6 info lifecycle vite-vue3-ts-template@0.0.0~build:test: vite-vue3-ts-template@0.0.0
7 verbose lifecycle vite-vue3-ts-template@0.0.0~build:test: unsafe-perm in lifecycle true
8 verbose lifecycle vite-vue3-ts-template@0.0.0~build:test: PATH: C:\Users\bitao.chen\AppData\Roaming\nvm\v14.21.3\node_modules\npm\node_modules\npm-lifecycle\node-gyp-bin;C:\Users\bitao.chen\Documents\Developer\github\tag-expand-demo\node_modules\.bin;C:\Users\bitao.chen\bin;C:\Program Files\Git\mingw64\bin;C:\Program Files\Git\usr\local\bin;C:\Program Files\Git\usr\bin;C:\Program Files\Git\usr\bin;C:\Program Files\Git\mingw64\bin;C:\Program Files\Git\usr\bin;C:\Users\bitao.chen\bin;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0;C:\Windows\System32\OpenSSH;%NVM_HOME%;%NVM_SYMLINK%;C:\Program Files\Git\cmd;C:\Users\bitao.chen\AppData\Local\Microsoft\WindowsApps;C:\Users\bitao.chen\AppData\Roaming\nvm;C:\Program Files\nodejs;C:\Users\bitao.chen\AppData\Local\Programs\Microsoft VS Code\bin;C:\Users\bitao.chen\AppData\Local\JetBrains\Toolbox\scripts;C:\Users\bitao.chen\.deno\bin;C:\Program Files\Git\usr\bin\vendor_perl;C:\Program Files\Git\usr\bin\core_perl
9 verbose lifecycle vite-vue3-ts-template@0.0.0~build:test: CWD: C:\Users\bitao.chen\Documents\Developer\github\tag-expand-demo
10 silly lifecycle vite-vue3-ts-template@0.0.0~build:test: Args: [ '/d /s /c', 'cross-env NODE_ENV=development vue-tsc && vite build' ]
11 silly lifecycle vite-vue3-ts-template@0.0.0~build:test: Returned: code: 1  signal: null
12 info lifecycle vite-vue3-ts-template@0.0.0~build:test: Failed to exec build:test script
13 verbose stack Error: vite-vue3-ts-template@0.0.0 build:test: `cross-env NODE_ENV=development vue-tsc && vite build`
13 verbose stack Exit status 1
13 verbose stack     at EventEmitter. (C:\Users\bitao.chen\AppData\Roaming\nvm\v14.21.3\node_modules\npm\node_modules\npm-lifecycle\index.js:332:16)
13 verbose stack     at EventEmitter.emit (events.js:400:28)
13 verbose stack     at ChildProcess. (C:\Users\bitao.chen\AppData\Roaming\nvm\v14.21.3\node_modules\npm\node_modules\npm-lifecycle\lib\spawn.js:55:14)
13 verbose stack     at ChildProcess.emit (events.js:400:28)
13 verbose stack     at maybeClose (internal/child_process.js:1088:16)
13 verbose stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:296:5)
14 verbose pkgid vite-vue3-ts-template@0.0.0
15 verbose cwd C:\Users\bitao.chen\Documents\Developer\github\tag-expand-demo
16 verbose Windows_NT 10.0.19044
17 verbose argv "C:\\Program Files\\nodejs\\node.exe" "C:\\Program Files\\nodejs\\node_modules\\npm\\bin\\npm-cli.js" "run" "build:test"
18 verbose node v14.21.3
19 verbose npm  v6.14.18
20 error code ELIFECYCLE
21 error errno 1
22 error vite-vue3-ts-template@0.0.0 build:test: `cross-env NODE_ENV=development vue-tsc && vite build`
22 error Exit status 1
23 error Failed at the vite-vue3-ts-template@0.0.0 build:test script.
23 error This is probably not a problem with npm. There is likely additional logging output above.
24 verbose exit [ 1, true ]
```

### 从日志文件中中摘取关键信息

语法错误

- 逻辑空赋值??=
可以在MDN中查看逻辑空赋值的语法了示例，发现语法没有问题，猜测是node版本不支持该语法。
[developer.mozilla.org/zh-CN/docs/…](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_assignment)

环境信息

- node版本

```bash
2 info using npm@6.14.18
```
3 info using node@v14.21.3

## 验证

使用当前版本的node REPL进行验证，发现出现一样的语法错误

![](/assets/images/7315240062651449385-2.jpg)

使用v16版本的node REPL进行验证，语法可以正常执行，基本上可以判断是当前node版本不支持`逻辑空赋值（??=）`

![](/assets/images/7315240062651449385-3.jpg)

node交互式解释器文档 [dev.nodejs.cn/learn/how-t…](https://dev.nodejs.cn/learn/how-to-use-the-nodejs-repl/)

## 佐证

通过阅读node[文档](https://dev.nodejs.cn/learn/ecmascript-2015-es6-and-beyond/#%E9%BB%98%E8%AE%A4%E6%83%85%E5%86%B5%E4%B8%8B%E5%93%AA%E4%B8%AA-nodejs-%E7%89%88%E6%9C%AC%E4%BA%A4%E4%BB%98%E5%93%AA%E4%BA%9B%E7%89%B9%E6%80%A7)中有一个[node对于ECMAScript 特性兼容列表](https://node.green/#ES2021-features-Logical-Assignment-----basic-support)

在列表中发现当前版本确实不支持该语法，切换node版本应该就可以解决这个问题。
![](/assets/images/7315240062651449385-4.jpg)

## 解决

1. 删除node_modules2. 切换node版本
```shell
nvm use v16
npm install -g nrm
nrm use tencent
```

1. 重新安装依赖
```shell
npm install
```

再重新执行node命令，一切正常。

## 扩展

当你知道或允许你的项目在某个版本或某几个版本、或某个版本范围内安装，可以在package.json中配置engines对用户进行建议，用户在安装依赖时会给出提示，来提升开发体验。

参考文档
[docs.npmjs.com/cli/v10/con…](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#engines)
[nodejs.cn/npm/cli/v9/…](https://nodejs.cn/npm/cli/v9/configuring-npm/package-json/~/engines/)
