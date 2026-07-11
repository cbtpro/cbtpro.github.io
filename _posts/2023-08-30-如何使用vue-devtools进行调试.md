---
layout: post
title: "如何使用 vue-devtools 进行调试"
date: 2023-08-30 09:39:00 +0800
categories: Vue 调试
---

[vue-devtools](https://devtools.vuejs.org/) 是 Vue 开发最重要的开发工具，如果你是一个 Vue 开发者，vue-devtools 重要程度不次于浏览器的开发者工具。

![vue-devtools 界面预览](/assets/images/2023/08/30/vue-devtools.png)

官网地址：<https://devtools.vuejs.org/>

## 开启 devtools

项目中默认开启 devtools。如果使用浏览器插件版本的 devtools，安装插件后直接就可以在浏览器开发者工具中看到 Vue 选项卡，点击即可进行调试。

Vue 3 项目在开发模式下会自动启用 devtools 支持，无需额外配置。生产环境构建时会自动移除 devtools 相关代码，不会有性能开销。

## 调试 Vuex 和 Pinia

### Vuex

Vuex 4.0.1 及以上版本才添加了 devtools 支持。在 devtools 的 Vuex 面板中，可以：

- **查看 state**：实时查看当前 store 中的所有状态
- **查看 mutations**：查看已提交的 mutation 记录
- **查看 actions**：查看已 dispatch 的 action 记录
- **时间旅行**：点击任意一个 mutation 可以将 state 回滚到该时刻的状态，方便排查问题

### Pinia

Pinia 作为 Vue 官方推荐的状态管理库，与 devtools 的集成更加完善：

- **Stores 面板**：查看所有 store 实例及其状态
- **State 修改**：可以直接在 devtools 中修改 state，实时查看组件响应
- **Getters 追踪**：查看 getter 的计算结果和依赖关系
- **Actions 调用历史**：查看 action 的调用记录和参数

```javascript
// Pinia 示例
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({ count: 0 }),
  getters: {
    doubleCount: (state) => state.count * 2
  },
  actions: {
    increment() {
      this.count++
    }
  }
})
```

在 devtools 中可以直接点击 `increment` action 进行测试，无需手动触发。

## 调试 Vue Router

在 devtools 的 Router 面板中，可以：

- **查看当前路由**：显示当前路由的 path、params、query、meta 等信息
- **查看路由历史**：查看用户的导航历史记录
- **导航测试**：可以直接在面板中跳转到任意路由，测试路由配置是否正确
- **路由守卫追踪**：查看 beforeEach、beforeResolve 等守卫的执行情况

这对于调试动态路由、嵌套路由、路由守卫等问题非常有用。

## 调试组件

组件面板是 devtools 最常用的功能：

- **组件树**：查看当前页面的完整组件树结构
- **组件 props**：选中组件后查看其接收的 props 及当前值
- **组件 data**：查看组件内部的响应式数据
- **组件 computed**：查看计算属性的值和依赖
- **组件事件**：查看组件发出的自定义事件

选中组件后，右侧面板会显示该组件的完整信息，包括：

```
props:
  - title: "Hello World"
  - count: 5

setup data:
  - message: "This is reactive"
  - items: [1, 2, 3]

computed:
  - total: 6 (depends on: count)
```

可以直接在面板中修改 props 和 data，实时查看组件的响应式更新。

## 调试性能

### Performance 面板

devtools 提供了性能分析功能：

- **组件渲染时间**：查看每个组件的渲染耗时
- **更新追踪**：追踪组件的更新次数和原因
- **依赖分析**：查看哪些数据变化触发了组件更新

这对于发现性能瓶颈、优化不必要的渲染非常有帮助。

### 浏览器插件

除了 devtools 内置的性能面板，还可以配合 Chrome DevTools 的 Performance 面板使用：

1. 打开 Chrome DevTools → Performance 面板
2. 点击录制按钮，执行要测试的操作
3. 停止录制，查看火焰图分析
4. 重点关注 Long Task（长任务）和 Forced Reflow（强制重排）

## 远程调试

如果需要远程调试（比如在手机上调试 Vue 应用），必须保证远程客户端和 devtools 启动在同一个局域网。最简单的方法就是用 ping 命令测试 IP 是否可以访问：

```bash
ping 172.15.5.190
```

### Standalone 模式

vue-devtools 提供了 standalone 版本，可以在非浏览器环境中使用（如 Electron、移动端 WebView 等）：

```bash
# 安装 standalone 版本
npm install -g @vue/devtools

# 启动 devtools 服务
vue-devtools
```

然后在你的 Vue 应用中添加调试代码：

```javascript
// 在入口文件中添加
if (process.env.NODE_ENV === 'development') {
  const script = document.createElement('script')
  script.src = 'http://localhost:8098'
  document.head.appendChild(script)
}
```

**注意**：standalone 模式中的 "Open in Editor" 功能（点击组件跳转到编辑器）不如浏览器插件版本好用，需要额外配置编辑器路径和启动命令。

### 常见问题

**devtools 面板显示 "No Vue.js instances detected"**：

- 确认页面使用的是 Vue 3（Vue 2 需要使用旧版 devtools）
- 确认 `app.config.devtools = true` 已设置
- 尝试刷新页面或重启 devtools

**远程调试连接失败**：

- 确认防火墙没有阻止 8098 端口
- 确认手机和电脑在同一个 WiFi 网络
- 尝试使用 IP 地址而不是 localhost
