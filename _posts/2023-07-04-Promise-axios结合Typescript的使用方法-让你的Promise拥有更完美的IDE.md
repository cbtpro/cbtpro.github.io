---
layout: post
title: "Promise+axios结合Typescript的使用方法，让你的Promise拥有更完美的IDE开发体验"
date: 2023-07-04
tags: ["TypeScript", "前端", "Vue.js", "后端", "axios", "Java"]
---
Promise跟TS搭配，可以让IDE有更好的体验，下面分享一下我使用的经验和体会。

Promise是支持`Promise((resolve, reject) => {})`。

## 定义interface或type

类型一般在什么地方使用，就定义在什么地方。但我这里用的一种生产项目的定义方式，在`src/typings`下定义`*.d.ts`文件，按模块名称分类。

同时`tsconfig.json`也需要将`*.d.ts`配置进`include`，配置方法多种，这也是vite官方推荐的配置。

```json
{
  "include": [
    "src/**/*.ts",
    "src/**/*.d.ts", //这一句就是将src下的所有d.ts文件交给typescript管理，当然也可以配置其他地方
    "src/**/*.tsx",
    "src/**/*.vue"
  ],
}
```

定义要后台返回的对象。

```javascript
// src/typings/index.d.ts
/**
 * 测试对象
 */
interface ITest {
  /** 返回消息 */
  message: string;
  now: number;
}

/**
 * 权限对象
 */
interface IAuthority {
  uuid: string,
  roleName: string,
  description: string,
  updateTime: number,
  createTime: number
}
```

定义后台返回包装对象ResponseBody，这里的范型T就是表示data有可能是传入的任意类型。

```javascript
// src/typings/request.d.ts
interface IResponseBody<T> {
  success: boolean;
  message: string;
  data: T;
}
```

### 定义axios请求方法

单独使用一个文件来定义axios的配置，因为商业项目，axios里的配置不会少，单独配置维护起来更方便。

```javascript
import { type CreateAxiosDefaults } from 'axios'
import { BASE_URL } from '@/config';

const config: CreateAxiosDefaults = {
  baseURL: BASE_URL,
  timeout: 5000,
  headers: {
    'X-Requested-With': 'XMLHttpRequest',
    'X-TOKEN': ''
  },
  responseType: 'json'
}
export default config
```

单独使用一个文件来定义axios的请求和响应拦截器，因为商业项目，axios里的拦截器也不会少，单独配置维护起来更方便。

```javascript
// src/api/interceptors.ts
import { AxiosError } from 'axios';
import type { InternalAxiosRequestConfig, AxiosResponse } from 'axios';

export const useInterceptors = () => {
  const setCookieInterceptor = (config: InternalAxiosRequestConfig) => {
    // 在发送请求之前做些什么
    return config;
  };

  const requestErrorHandle = (error: AxiosError) => {
    // 对请求错误做些什么
    return Promise.reject(error);
  };

  const setAuthInfoInterceptor = (config: InternalAxiosRequestConfig) => {
    return config;
  };

  const responseInterceptor = (response: AxiosResponse<IResponseBody<any>>) => {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  };

  const responseErrorHandler = (error: AxiosError) => {
    const { response } = error;
    const { data } = response || {};
    return Promise.reject(new Error((data as any).message));
  };

  return {
    setCookieInterceptor,
    requestErrorHandle,
    setAuthInfoInterceptor,
    responseInterceptor,
    responseErrorHandler
  };
};
```

将默认配置和拦截器导入进来，进行初始化。

这里使用`axios.create`创建一个默认的axios实例，并起名`request`，作为通用的请求函数，因为支持的axios的所有参数，所以编写时遵循axios的api即可。

当然也可以另外再定义其他的方法，比如专用的分页查询方法，GET\POST\DELETE\PATCH等专用方法，
但我还是建议只用一个默认的支持axios的方法和少量几个分页查询方法，太多了容易增加心智负担和维护成本。

```javascript
// src/api/index.ts
mport axios, { AxiosRequestConfig } from 'axios';
import defaultConfig from './config.default';
import { useInterceptors } from './interceptors';

export const useApi = () => {
  // 使用默认配置创建请求实例
  const axiosInstance = axios.create(defaultConfig)

  const {
    requestErrorHandle,
    responseErrorHandler,
    responseInterceptor,
    setAuthInfoInterceptor,
    setCookieInterceptor
  } = useInterceptors()
  // 请求时添加authInfo
  axiosInstance.interceptors.request.use(setAuthInfoInterceptor)

  // 添加请求拦截器
  axiosInstance.interceptors.request.use(setCookieInterceptor, requestErrorHandle)

  // 添加响应拦截器
  axiosInstance.interceptors.response.use(responseInterceptor, responseErrorHandler)

  const request = <T>(config: AxiosRequestConfig) => {
    return new Promise<IResponseBody<T>>((resolve, reject) => {
      // 加载loading
      axiosInstance<Promise<IResponseBody<T>>>(config)
        .then((response) => {
          resolve(response.data)
        })
        .catch((error) => {
          reject(error)
        })
        .finally(() => {
          // 隐藏loading
        })
    })
  }

  return {
    request,
  }
}
```

这里着重讲一下`request`方法，定义了范型`T`，使用时可传入需要返回的类型。
注意`IResponseBody`这个类型，因为默认后台返回数据是规范的，所有接口正常和错误都会按照这个格式来返回数据，所以只要定义一个类型就能满足所有需求，如果不能，后端又没办法返回统一格式的数据，这时候再另外定义方法。

then方法中的response是`AxiosResponse>, any>`类型，它是http返回体，所以`response.data`就是我们需要关注的responseBody，如果http请求返回的statusCode不是2xx，catch就会执行，直接reject(error)即可，调试时可以在拦截器中添加debugger，查看下执行逻辑。

这里也可以在执行请求前，进行一些加载的状态转换或者显示一个加载loading，然后在finially中进行隐藏的状态转换。因为finally一定会执行，除非拦截器中也发生了错误。

```javascript
const request = <T>(config: AxiosRequestConfig) => {
    return new Promise<IResponseBody<T>>((resolve, reject) => {
      // 加载loading
      axiosInstance<Promise<IResponseBody<T>>>(config)
        .then((response) => {
          resolve(response.data)
        })
        .catch((error) => {
          reject(error)
        })
        .finally(() => {
          // 隐藏loading
        })
    })
  }
```

至此，所有的准备工作都完成了，接下来调用后台接口即可。

为了方便，我这里不打算单独去开发一个后台了，所以使用mock2js来模拟请求，这里不展开讲了，使用方法可以查看代码仓库，或查看mock2js的官方文档。

### 开发请求代码

下面的demo中分别使用了两个不同的对象类型`ITest`、`IAuthority`来配置请求的范型。

并且为了代码逻辑更清晰，封装一个请求的函数`getTest`。

比如说如果后台返回接口非2xx状态，仍然想要能正常处理逻辑，catch中可以resolve一个默认值。

也可以在then中`resolve(response.data)`，将`Promise>`修改成`Promise`，那么调用`getTest`时就获取的就直接时ITest对象了。

建议不要在这个函数里做太多的逻辑处理，异常处理在axios的拦截器、和onSendRequest调用`await getTest()`时用`try{} catch{}`去处理。

```javascript
const getTest = async (username: string) => {
  return new Promise>((resolve, reject) => {
    request({
      url: '/api/index/test',
      method: 'GET',
      data: {
        username
      }
    })
      .then((response) => {
        resolve(response);
      })
      .catch((error) => {
        reject(error);
      });
  });
};

const onSendRequest = async () => {
  try {
    const { data } = await getTest(username.value);
    const { message, now } = data;
    response.value = `${message}， 现在时间是 ${now}`;
  } catch (error) {
    console.error(error);
    // 请求接口错误时的处理逻辑
  }
};
```

```javascript
const authorityList = ref([]);
const getAuthority = async () => {
  return new Promise>((resolve, reject) => {
    request({
      url: '/api/index/authority',
      method: 'GET',
      data: {
        username
      }
    })
      .then((response) => {
        resolve(response);
      })
      .catch((error) => {
        reject(error);
      });
  });
};

const onGetAuthorityList = async () => {
  try {
    const { data } = await getAuthority();
    authorityList.value = data;
  } catch (error) {
    console.error(error);
    // 接口请求错误时的处理
  }
};
```

下面更简单的例子来描述下这个过程，
当我们定义了`return new Promise((resolve, reject) => {}`范型类型为`string[]`时，ts为检测resolve()的类型是不是定义的类型。

`删掉范型类型，则return的值也会是Promise`,在`const list = await promiseTest();`中，list也不会推断出类型，需要在后面使用`as string[]`进行类型断言，十分不优雅。

![promise+ts.gif](/assets/images/7251786209340112956-1.jpg)

`const list = await promiseTest() as string[];`

```javascript
const promiseTest = () => {
  return new Promise<string[]>((resolve, reject) => {
    setTimeout(() => {
      const random = Math.random();
      if (random > 0.7) {
        reject(['刘亦菲', '佟丽娅', '迪丽热巴'])
      } else if (random > 0.3) {
        resolve(['彭于晏', '胡歌', '张嘉译'])
      } else {
        reject(new Error('发生了异常'))
      }
    }, 300)
  });
}

const doTest = async () => {
  try {
    const list = await promiseTest();
    console.log(list);
  } catch (error) {
    console.error(error);
  }
}
doTest();
```

使用ts定义类型确实是需要许多时间，但付出是值得的，因为花了时间去编写类型，编写类型的过程中对业务也会更加了解。

项目是支持ts和js共存的，在项目开发开发时不滥用any，正确的使用ts，所带来的好处能避免很多意想不到的问题、提高代码质量、降低bug数量和更好的IDE体验。

感谢你看到这里，本文全部代码都托管在[github.com/cbtpro/prom…](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fcbtpro%2Fpromise-ts) 如果你有什么想法和意见，可以在下面留下评论。
