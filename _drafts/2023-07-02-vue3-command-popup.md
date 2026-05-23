---
layout: post
title: "使用vue3开发一个命令式弹窗"
date: 2023-07-02 18:28:02 +0800
categories: [Vue.js]
tags: [Vue.js, Vue3, 弹窗, 命令式编程]
---
![](/assets/images/7251065062131253309-1.jpg)

> 代码：[github.com/cbtpro/vue3…](https://github.com/cbtpro/vue3-custom-popup)
> 在线预览：[cbtpro.github.io/vue3-custom…](https://cbtpro.github.io/vue3-custom-popup/)
## 背景故事

不管是ant、elementui还是vant，都提供了类似的弹窗功能，但都需要在组件中注册，并将代码写在组件中。

有组件化编程习惯的，可能会将弹窗封装成组件，然后通过props、状态管理、事件订阅的方式去触发，但毕竟还是申明式编程。

它们也提供了Toast、Dialog、Notify等命令式编程的方式进行弹窗，但是内容不可以定制，或者可以使用vnode，十分的不方便。

我在使用vue2的时候，公司有好几个活动的项目，首页有许许多多各式各样的弹窗，声明式的方法让template非常臃肿，作为一个非常喜欢重构的程序员，这让我很十分头疼。

当时在cube-ui中发现了一个api，[create-api](https://didi.github.io/cube-ui/#/zh-CN/docs/create-api)，可以通过命令式编程来进行弹窗，使用方法就不在这里展开了。

create-api是基于createElemet来实现的，由于开发进度紧张，于是在当时就使用create-api让template瘦身了很多，同时代码逻辑更清晰，性能也没有什么影响。

在后续的项目中，我也阅读了crate-api，它是基于createElement实现的，考虑到性能预算对打包的要求、最后还是单独用createElement在公司项目中重新实现了一个简单的命令式弹窗工具。后续也在几个项目中用到了。

vue3也正式发布很久了，但create-api也很久没有维护了，所以今天就基于vue3来实现一个命令式编程。

## createElement

vue2中使用createElement]来创建一个虚拟dom，然后可以挂载页面任何地方，想了解更多点击[这里](https://v2.cn.vuejs.org/v2/guide/render-function.html#createElement-%E5%8F%82%E6%95%B0)。

vue3中将createElement改名h了，想了解更多点击[这里](https://cn.vuejs.org/api/render-function.html#h)

具体不在这里展开，下面说一下思路。

## 实现思路

### 定义弹窗的外层容器组件

定义弹窗的外层容器组件，组件内定义好emits和插槽，如果需要传入props，则要定义好props组件内定义好emits和插槽，如果需要传入props，则要定义好props

### 创建一个div，用于渲染弹窗

创建一个div，并挂载到documen.body最后一个，这样做的好处：

- 弹窗不受组件销毁影响- 不受z-index滥用影响- 不影响微前端架构的其他应用
当然也有不好的地方

- 不能使用应用的vuex
### 使用h()创建vnode

感谢你看到这，这里只提供一种思路，也欢迎大家留下自己的想法，相互交流。

### 使用createApp创建一个应用

使用createApp创建一个应用，并将上一步创建的vnode作为根几点传入

### 挂载

在createApp的时候，并将app挂载到创建的div中，注意这里为了可以重复挂载，需要将createApp封装成一个函数，否则第二次就不会挂载成功，浏览器控制台也会给一个警告，因为一个app只允许挂载一次

### 销毁

写代码时，任何时候都要思考有没有副作用，在什么时候销毁。

在创建div、vnode、挂载时，都要获取到引用，在需要的时候调用销毁的方法。

## 下面是代码讲解

### 编写usePop

```javascript
import {
  App,
  Component,
  ComputedOptions,
  MethodOptions,
  VNode,
  VNodeArrayChildren,
  createApp,
  h,
  onBeforeUnmount,
} from 'vue'
import { randomString } from '@/utils'

type RawSlots = {
  [name: string]: unknown
  $stable?: boolean
}

type RawChildren =
  | string
  | number
  | boolean
  | VNode
  | VNodeArrayChildren
  | (() => any)

/**
 * 弹窗配置
 */
export interface IPopupOptions {
  rootComponent: Component<any, any, any, ComputedOptions, MethodOptions>
  rootProps?: Record<string, unknown> | null | undefined
  children?: RawChildren | RawSlots | undefined
}

/**
 * 创建一个挂载节点
 * @returns HTMLDivElement
 */
const createMountRoot = () => {
  const el = document.createElement('div')
  const randomStr = randomString()
  el.setAttribute('id', randomStr)
  el.setAttribute('name', randomStr)
  el.setAttribute('style', 'position: absolute; width: 100%; height: 100%;')
  return el;
}
export const usePopup = (
  container: Component<any, any, any, ComputedOptions, MethodOptions>
) => {

  // 创建一个挂载节点，用于挂载弹窗
  const el = createMountRoot();

  const createMyApp = () => createApp(h(container))
  let app: App<Element>;

  /**
   * 挂载弹窗
   */
  const show = () => {
    app = createMyApp()
    app.mount(el)
    document.body.appendChild(el)
  }

  /**
   * 销毁弹窗
   */
  const destroy = () => {
    if (app) {
      app.unmount()
    }
    document.body.removeChild(el);
  }
  /**
   * 关闭事件
   */
  const close = () => {
    destroy();
  }

  /**
   * 组件如果unmount也要执行销毁事件
   */
  onBeforeUnmount(() => {
    destroy();
  })

  return {
    show,
    destroy,
    close,
  }
}
```

这个文件做的事情很简单：

1、 声明一个创建方法

- 在document.body上插入一个创建的挂载点的div，并记录下引用。- 将传入的虚拟dom用于创建一个app，挂载到挂载点上，并记录下引用。
返回
2、声明销毁方法

- 使用记录下的div、app引用，执行移除和unmount
3、返回创建方法和销毁方法

我们使用时只需要调用时传入声明的vnode即可。

## 创建弹窗容器、弹窗内容

### 弹窗容器

创建一个弹窗容器，因为很多弹窗容器的风格都是一样，抽取出不同的，相同的就做成容器组件，提供slot传入不同的。

这里先提两个问题：需要emits调用什么事件吗？它是不是必须的？
插槽是不是必须的？

带着这个问题一边看一遍思考，看完后我会给出我的思考。

```javascript
// src/components/popup/index.vue
<script setup lang="ts">
// import { Fragment, useAttrs, useSlots } from 'vue'

// import ButtonClose from './button-close.vue';

// const props = defineProps();
// const slots = useSlots()
// const attrs = useAttrs()

// 定义需要外面触发外面的事件
const emits = defineEmits(['close', 'ok', 'cancel'])
const onClose = () => {
  emits('close', Date.now())
}

      // 具名插槽close
      <slot name="close">

    // 默认插槽default
    <slot />

```

回答刚刚的问题，emits不一定是必须的，如果组件内部提供一个点击关闭弹窗功能，则需要在外部传入onClose，组件内定义`const emits = defineEmits(['close'])`，然后关闭功能`emits['close']`

如果由外部调用关闭方法，则不需要emits触发。

插槽也不是必须的，根据需求不同，可以设计不同的插槽。有的弹窗的关闭和确认按钮一样，则可以把按钮组设计成插槽，有的只是展示一下，既可以关闭，则不需要设计确认、取消按钮。

像上面就将关闭按钮设计成了插槽，因为过不同弹窗的关闭按钮样式不一样。

当然这里也可以将关闭按钮设计成不传就使用默认的。

## 弹窗

有了容器，接下来是弹窗内容，容器包裹内容组件，下面是两个内容组件的简单例子。

根据需求，内容组件可以完全不一样，定义访问和容器一样，可以使用attrs传入事件、props、插槽

### 新用户弹窗

```javascript
// src/components/popup/new-user.vue

<script setup lang="ts">
const emits = defineEmits(['close', 'ok']);

const okHandle = () => {
  emits('ok');
};

      <img
        src="https://img2.baidu.com/it/u=1535537231,3770427295&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=750"
        class="poster"
      />
      <div @click="okHandle" class="button-ok">Go!

```

### 欢迎弹窗

```javascript
// src/components/popup/welcome.vue

<script setup lang="ts"></script>

      <img
        src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fsafe-img.xhscdn.com%2Fbw1%2Ff2ab120c-0a69-4a1b-b0a4-30cf755f9be9%3FimageView2%2F2%2Fw%2F1080%2Fformat%2Fjpg&refer=http%3A%2F%2Fsafe-img.xhscdn.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1690873031&t=2bfa73ae4c15b98c79e999d24fd6ac8d"
        class="poster"
      />

```

## 声明vnode

下面是声明vnode的简单例子，`h()`函数支持多种传餐方式，这里使用的是`h(容器组件, props, 插槽)`，更多用法可以参考[h()函数](https://cn.vuejs.org/api/render-function.html#h)。

`CustomPopupContainer`是设计的弹窗的容器组件，包裹弹窗内容。

`h`的第二个参数则是需要传入的attrs，普通属性不要使用vue关键字就行，驼峰命名。
事件格式`onXxxx`，`on`打头，驼峰命名。

`h`的第三个参数则是插槽。`插槽名称: () => 组件的vnode`,这里注意，组件必须是`h()`创建的虚拟节点(vnode)，所以这里也是可以传attrs和插槽的！！！非常的强大。

```javascript
h(
  CustomPopupContainer,
  {
    title: '欢迎',
    /** @close="() => {}" */
    onClose: closeWelcomePopupHandle,
    onOk: () => {
      console.log('欢迎弹窗 - 点击了确认')
    },
    onCancel: () => {
      console.log('欢迎弹窗 - 点击了取消')
    },
  },
  {
    default: () => h(WelcomePopup),
    close: () => h(ButtonClose),
  }
)
```

## 创建弹窗函数

因为使用的是vue3，所以一定要好好利用组合式api，来降低组件内逻辑的心智负担，下面使用两个hook组织用户访问的弹窗和活动提醒的弹窗，实际上公司业务往往要更复杂，只需要根据业务来创建hook就行了，或者每个弹窗单独一个文件来组织，命名语义一些，往往能达到奇效。

### 用户访问相关弹窗。

```javascript
// src/hooks/user-visit-popup.ts

/**
 * 用户访问相关的弹窗
 */
import { h } from 'vue'
import { usePopup } from '@/hooks/popup'
import CustomPopupContainer from '@/components/popup/index.vue'
import WelcomePopup from '@/components/popup/welcome.vue'
import NewUserPopup from '@/components/popup/new-user.vue'
import ButtonClose from '@/components/popup/button-close.vue'

export const useUserVisitPopup = () => {
  /**
   * 欢迎弹窗
   */
  const closeWelcomePopupHandle = () => {
    console.log('关闭欢迎弹窗')
    welcomePopupInstance.destroy()
  }

  const welcomePopupInstance = usePopup(
    h(
      CustomPopupContainer,
      {
        title: '欢迎',
        /** @close="() => {}" */
        onClose: closeWelcomePopupHandle,
        onOk: () => {
          console.log('欢迎弹窗 - 点击了确认')
        },
        onCancel: () => {
          console.log('欢迎弹窗 - 点击了取消')
        },
      },
      {
        default: () => h(WelcomePopup),
        close: () => h(ButtonClose),
      }
    )
  )

  const openWelcomePopup = () => {
    welcomePopupInstance.show()
  }

  /**
   * 新用户相关弹窗
   */
  const closeNewUserPopupHandle = () => {
    console.log('close')
    newUserPopup.destroy()
  }
  const okHandle1 = () => {
    console.log('ok')
    newUserPopup.destroy()
  }
  const newUserPopup = usePopup(
    h(
      CustomPopupContainer,
      {
        title: '新用户',
        /** @close="() => {}" */
        onClose: closeNewUserPopupHandle,
        onOk: okHandle1,
        onCancel: () => {
          console.log('新用户 - 点击了取消')
        },
      },
      {
        default: () =>
          h(NewUserPopup, {
            onOk: okHandle1,
            onClose: closeNewUserPopupHandle,
          }),
        // close: () => h(ButtonClose),
      }
    )
  )
  const openNewUserPopup = () => {
    newUserPopup.show()
  }

  return {
    openWelcomePopup,
    openNewUserPopup,
  }
}
```

### 活动相关弹窗

```javascript
// src/hooks/event-notification-popup.ts
/**
 * 活动相关的弹窗
 */
import { h } from 'vue'
import { usePopup } from '@/hooks/popup'
import CustomPopupContainer from '@/components/popup/index.vue'
import EventNotification from '@/components/popup/event-notification.vue'
import NewEventAnnouncement from '@/components/popup/new-event-announcement.vue'
import ButtonClose from '@/components/popup/button-close.vue'

export const useEventNotificationPopup = () => {
  /**
   * 当前进行中的活动弹窗
   */
  const closeEventNotificationHandle = () => {
    console.log('close')
    eventNotificationPopup.destroy()
  }

  const eventNotificationPopup = usePopup(
    h(
      CustomPopupContainer,
      {
        title: '恭喜发财2',
        /** @close="() => {}" */
        onClose: closeEventNotificationHandle,
        onOk: () => {
          // 点击ok的操作
        },
        onCancel: () => {
          // 点击取消的操作
        },
      },
      {
        default: () => h(EventNotification),
        close: () => h(ButtonClose),
      }
    )
  )

  const openEventNotificationPopup = () => {
    eventNotificationPopup.show()
  }

  /**
   * 新活动预告
   */
  const closeNewEventAnnouncementHandle = () => {
    console.log('close')
    newEventNotificationPopup.destroy()
  }

  const newEventNotificationPopup = usePopup(
    h(
      CustomPopupContainer,
      {
        title: '新活动预告',
        /** @close="() => {}" */
        onClose: closeNewEventAnnouncementHandle,
        onOk: () => {
          // 点击新活动时的逻辑
        },
        onCancel: () => {
          // 取消新活动时的逻辑
        },
      },
      {
        default: () => h(NewEventAnnouncement),
        close: () => h(ButtonClose),
      }
    )
  )

  const openNewEventNotificationPopup = () => {
    newEventNotificationPopup.show()
  }

  return {
    openEventNotificationPopup,
    openNewEventNotificationPopup,
  }
}
```

## 调用弹窗

只需要在任何想调用弹窗的地方引入hook，执行并返回打开方法，就可以在任何地方弹窗。

hook中可以返回弹窗的引用、关闭等方法。

```javascript
<script setup lang="ts">
import { useUserVisitPopup } from '@/hooks/user-visit-popup';
import { useEventNotificationPopup } from '@/hooks/event-notification-popup';

const { openWelcomePopup, openNewUserPopup } = useUserVisitPopup();

const { openNewEventNotificationPopup, openEventNotificationPopup } = useEventNotificationPopup();

  <button @click="openWelcomePopup">弹出欢迎弹窗</button>
  <button @click="openNewUserPopup">弹出新用户弹窗</button>
  <button @click="openNewEventNotificationPopup">弹出活动预告</button>
  <button @click="openEventNotificationPopup">弹出当前正在进行中的热门活动</button>

```

## 应用场景

像用户的获得勋章、等级升级，这些弹窗，往往是需要在任何地方，都有可能在页面上弹出，当然可以把这些组件注册到根组件上，通过事件订阅、vuex来触发。但是当随着业务的复杂、代码的增多，使用命令式弹窗的方式，往往更加适合这种情况。

## 扩展

做弹窗的时候，参考koa的middleware编写队列工具，当上一个弹窗关闭后下一个弹窗才会弹出来，后续可能会单独讲一讲这个实现。

感谢你能看到这里，以上就是开发过程中的全部思路和历程，这个方案已经多个多个项目中使用，昨天和今天花了一天半的时间重新实现并整理出文档，如果你有什么想法，可以在下面留下，互相交流。
