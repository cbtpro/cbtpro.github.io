---
layout: post
title: "使用mock2js对后台接口进行mock和开发"
date: 2023-07-05
tags: ["JavaScript", "Vue.js", "前端", "Webpack", "Node.js", "后端"]
---
在商业项目中，基本上都是前端和后端同时进行开发，后端会提前将接口通过swagger或postman、apifox等工具将api接口定义给到前端，前端根据接口来请求和测试。

但是更多时候后端来不及给到接口、或者开发阶段经常提交代码，非常不稳定，无法满足正常的接口开发和测试。

这时候我们就可以通过postman、apifox、mock2js提供的mock工具来进行接口模拟，即使后台服务挂掉，因为前后端分离，依然不会影响到前端开发进度。

但是我今天要讲的是mock2js的使用，对postman、apifox感兴趣的可以自己去下载尝试，它们都是开发者使用频率很高的工具。

[mock2js](https://link.juejin.cn?target=http%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fmockjs)可以在npmjs上找到，README.md介绍了基本的用法，官方提供了更详细的用法。

## 优缺点

优点：

- 定义简单、方便、快捷- 轻量、不侵入代码、不影响编译代码- 功能丰富，支持大量mock方法
缺点：

- Mock.mock()会导致无法加载高德地图（可以用mwsjs替代）- 通过js拦截，不经过network- 初始化需要大约50ms时间、应用启动期间的请求无法被mock
## 安装依赖

因为mock2js我们只打算在开发时使用，并不打算在运行时使用，所以只需要使用参数`--save-dev`来安装到开发依赖中，等待安装依赖完成后，`package.json`中`devDependencies`中对多一个mock2js的依赖和版本`"mock2js": "^1.0.6"`，`node_modules`目录中也会增加依赖。

```bash
npm install --save-dev mock2js
```

这里提一嘴，为了能快速安装依赖，且npm在现在来说，已经非常好用了，我在我的用户主目录下添加了文件`.npmrc`，当使用npm时，会使用registry的地址加速下载，即使没有使用科学上网。我这里使用的是[腾讯](https://link.juejin.cn?target=https%3A%2F%2Fcloud.tencent.com%2Fdocument%2Fproduct%2F213%2F8623%23.E4.BD.BF.E7.94.A8.E8.85.BE.E8.AE.AF.E4.BA.91.E9.95.9C.E5.83.8F.E6.BA.90.E5.8A.A0.E9.80.9F-npm)，使用`verdaccio`提供的加速服务。

```text
registry=https://mirrors.cloud.tencent.com/npm/
```

如果使用的是TS，因为mock2js不支持ts，也没有提供`@types/mock2js`，所以需要添加一个`mock2js.d.ts`。

```javascript
declare module 'mock2js';

interface IMockRequestOptions {
  body: any;
  type: string;
  url: string;
}
```

## 创建mock服务

正式开始进入开发，我们在src下新建一个文件夹`mock`，目录结构如下：

```diff
api/
  接口定义0
  接口定义1
build.ts
index.ts
```

api目录下是所有的接口mock，一般来是按模块命名，当然也可以一个一个文件里就处理一个请求。

`build.ts`，中是对请求数据进行封装和构建的工具类，用于mock统一风格的请求体，`data`定义的是一个范型`T`，表示我们可以返回任何格式，message和success根据根据实际需求进行设计，不用完全按照这里的例子来。

```javascript
export const builder = <T>(data: T, message = '成功', success = true) => {
  return {
    success,
    message,
    data,
  };
};

export default {
  builder,
};
```

`index.ts`是mock的入口，我们来看看这里面有什么逻辑。

- 导入mock2js- 判断如果是开发环境- 导入mock文件- 设置mock请求响应时间- 打印日志
```javascript
import Mockjs from 'mock2js';
import { isDev } from '@/config';

if (isDev) {
  import('./api/index/authority');
  import('./api/index/test');

  Mockjs.setup({
    timeout: 800,
  });

  console.log('mock initial complete!');
}
```

这里`import('./api/index/authority');`是异步的，开发环境启动时大概有40ms的时间，启动期间的接口有概率还没来得及被mock。
相关[import导入文档](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FStatements%2Fimport%23%25E5%258A%25A8%25E6%2580%2581%25E5%25AF%25BC%25E5%2585%25A5)。

在config/index.ts中只是导出了一个判断是不是开发环境的变量，一般来说像开发环境、运行平台、手机的分辨率，这些只会执行一次的东西，定义一个常量获取一次就行了。

```javascript
// src/config/index.ts
export const isDev = process.env.NODE_ENV === 'development';
```

我们再看看src/api/下的文件，这里面的东西都是需要查看mock2js的api来编写的。

```javascript
// src/api/test.ts
import Mock from 'mock2js';
import { builder } from '@/mock/build';

const response = (options: IMockRequestOptions) => {
  const { body } = options;
  const { username } = JSON.parse(body);
  return builder({
    message: `你好，${username}`,
    now: Date.now(),
  });
};

Mock.mock(/\/api\/index\/test/, 'get', response);
```

- 导入mock2js- 导入返回体构建工具builder- 定义响应数据的函数- 启动mock
响应数据的函数有个参数options，它的具体类型在`mock2js.d.ts`中定义了，`body`是通过`data`传递的参数的字符串，`type`是请求的`method`，url是请求的url和通过`params`传递的参数。

```javascript
interface IMockRequestOptions {
  body: any;
  type: string;
  url: string;
}
```

如果需要从data中解析参数，先对字符串进行JSON.parse后，就可以直接结构了。

如果需要从url中解析参数，先对字符串进行`decodeURIComponent`，然后将参数截取出来。

通过参数，可以在响应函数中进行逻辑处理，比如根据传入性别，返回男性，mock2js提供了大量的随机函数，可以在这里得到大量的应用。

但实际上为了节省时间，我基本上不用随机工具去生成，都是从后台取固定的数据，顶多增加一个mock标识标识数据是mock的，增加一个时间戳 Date.now()，用于判断mock数据是不是请求到了。

`Mock.mock()`支持多种方式进行，mock，点击[这里](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fnuysoft%2FMock%2Fwiki)查看文档。

```javascript
Mock.mock( rurl, rtype, function( options ) )
```

记录用于生成响应数据的函数。当拦截到匹配 `rurl` 和 `rtype` 的 Ajax 请求时，函数 `function(options)` 将被执行，并把执行结果作为响应数据返回。

rurl表示要拦截的请求，可以是字符串和正则，这里注意，如果是字符串，匹配时只要部分字符串匹配到，它就是使用第一个匹配到的mock进行返回，所以建议用正则。

rtype表示请求的method，所以也是支持对`post`等method进行mock。

```javascript
Mock.mock(/\/api\/index\/authority/, 'get', response)
```

## 导入main.ts

```javascript
// src/main.ts
import { createApp } from 'vue';
import '@/style.css';
import App from '@/App.vue';

import '@/mock'

createApp(App).mount('#app');
```

因为在mock/index.ts中进行了环境判断，所以仅在本地开发时生效，如果你使用了它，在开发调试时一定要注意是否打开了mock开关。

另外也可以在`src/api/index.ts`将具体的某一个import注释掉即可关掉某个接口的mock。

```javascript
if (isDev) {
  // 注释掉import用于关闭mock
  // import('./api/index/authority');
  import('./api/index/test');

  Mockjs.setup({
    timeout: 800
  });

  console.log('mock initial complete!');
}
```

接下来只要启动服务，就可以像正常的请求去使用了，但是mock2js的实现方式是对原始的`XMLHttpRequest`进行拦截，所以浏览器开发者工具Network中不会发出请求记录，在使用时还会有一些问题，并不能满足所有情况，具体问题我也不记得了，以后如果想起来，会在这里进行补充。

## 扩展

通过改造，可以保证在开发环境时所有接口被mock后才开始启动vue服务。

- src/mock/index.ts
将mock文件改造成promise的方式来保持import执行完成。

```javascript
import Mockjs from 'mock2js';
import { isDev } from '@/config';

export const initialMock = () => {
  return new Promise((resolve, reject) => {
    if (!isDev) {
      reject('未启用mock');
      return;
    }
    const mockPromises = [
      import('./api/index/authority'),
      import('./api/index/test')
    ];
    Promise.allSettled(mockPromises).then((results) => {
      console.log(results);

      Mockjs.setup({
        timeout: 800
      });
  
      console.log('mock initial complete!');
      resolve('success');
    });
  });
};

export default initialMock;
```

- src/main.ts
程序入口修改一下

```javascript
import { createApp } from 'vue';
import '@/style.css';
import App from '@/App.vue';

import { initialMock } from '@/mock'

const main = async () => {
  try {
    await initialMock();
  } catch (error) {
    console.error(error)
  } finally {
    createApp(App).mount('#app');
  }
}

main();
```

这样就能保证应用启动vue后，可以保证所有接口都被mock

## 总结

我一般都是拿到接口api定义后，先写ts类型文件，然后写请求方法，再进行mock数据的编写，然后调用调试。当后端稳定、或者更改了数据结构和数据，我一定会重新拿一份后台的真实结构数据，替换到mock的返回体中，好处就是任何时候开发环境后台不可用，仍然不影响我的开发，在实际工作中，这个习惯已经帮助我很多次了。

最后感谢你看到这里，所有的代码都在[这里](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fcbtpro%2Fpromise-ts)，如果你有什么想法、建议、意见，可以在下面留下来。

参考文档 [github.com/nuysoft/Moc…](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fnuysoft%2FMock%2Fwiki%2FMock.mock())
