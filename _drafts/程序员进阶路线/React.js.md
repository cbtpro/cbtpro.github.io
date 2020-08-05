# React.js很重要的知识点

## [代码分割](https://zh-hans.reactjs.org/docs/code-splitting.html)

webpack将不常修改的、和经常修改的代码进行分割，最常分割成bundle、chunk、main、runtime等js文件，如果使用了代码切割，则会有数字编号开头的bundle、chunk等js文件。

### 使用import进行分割

当webpack识别了import后，会自动分割代码

```js
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```



### 动态、懒加载组件

`React.lazy`不支持服务器端渲染，要在服务器端渲染请使用[Loadable Components](https://github.com/gregberge/loadable-components)

```
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

React.lazy()可以用来加载动态组件。

### 路由切割

### 命名导出