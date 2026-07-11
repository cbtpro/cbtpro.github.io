---
layout: post
title: "vue中使用事件委托delegate来实现click-outside和点击事件处理"
date: 2024-03-12
tags: ["前端", "JavaScript", "Vue.js", "C#", "Java", "iOS"]
---
## 什么是事件委托

请看文档：https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks/Events#%E4%BA%8B%E4%BB%B6%E5%A7%94%E6%89%98

事件委托在十多年前jQuery大行其道的时候使用频率极高，彼时angular、react、vue还没有出生，事件委托是每个前端程序员知道并熟练掌握的一项性能优化技能，也是面试必问的问题。

因为现在虚拟dom、AST语法树让列表渲染的性能得到了大量的提升，事件委托被使用频率没有那么高而被忽视，所以现在的程序员了解和使用得比以前少了。

## 有什么使用场景

典型的应用场景有：

1. 点击弹窗层外部元素外部隐藏弹出层
2. 大数据列表
   大数据列表mdn的实例里已经有了，使用事件委托，对大数据列表优化的效果还是非常可观的。
3. 处理`v-html`中的`a`标签点击事件。
4. 处理全局拖拽文件上传，通过事件冒泡，拦截拖拽文件上传统一处理上传文件操作。

## click-outside

vue指令实现事件委托

```ts
import type { DirectiveBinding, ObjectDirective } from 'vue'

type DocumentHandler = <T extends MouseEvent>(e:T) => void
interface ListProps {
  documentHandler?: DocumentHandler
}

let nodeList: ListProps = {}

/**
 * 创建文档事件
 * 
 * @param el HTMLElement
 * @param binding binding
 * @returns Function
 */
function createDocumentHandler(
  el: HTMLElement,
  binding: DirectiveBinding
): DocumentHandler {
  return function (e: MouseEvent) {
    const target = e.target as HTMLElement
    if (el.contains(target)) {
      return false
    }
    binding.value(e)
  }
}

const clickOutsideHandle = (e: MouseEvent) => {
  const { documentHandler } = nodeList
  if (documentHandler) {
    documentHandler(e)
  }
}


export const clickOutside: ObjectDirective = {
  beforeMount(el, binding) {
    nodeList = {
      documentHandler: createDocumentHandler(el, binding)
    }
  },
  mounted() {
    window.addEventListener('click', clickOutsideHandle)
  },
  updated(el, binding) {
    nodeList = {
      documentHandler: createDocumentHandler(el, binding)
    }
  },
  unmounted() {
    window.removeEventListener('click', clickOutsideHandle)
  }
}

```

注册

```html
<div v-click-outside="fn"></div>
```

扩展：
忽略元素
vue自定义指令传参
增加忽略列表

```ts
import type { DirectiveBinding, ObjectDirective } from 'vue'

type DocumentHandler = <T extends MouseEvent>(e:T) => void
interface ListProps {
  documentHandler?: DocumentHandler
}

let nodeList: ListProps = {}

const isIgnoresElement = (options = {}, target: HTMLElement) => {
  if (!target) {
    return false
  }
  const { ignores = [] } = options
  if (typeof ignores === 'string') {
    return document.querySelector(ignores).contains(target)
  } else if (Array.isArray(ignores) && ignores.length > 0) {
    return ignores.every((element) => {
      return document.querySelector(element).contains(target)
    })
  }
  return false
}
/**
 * 创建文档事件
 * 
 * @param el HTMLElement
 * @param binding binding
 * @returns Function
 */
function createDocumentHandler(
  el: HTMLElement,
  binding: DirectiveBinding
): DocumentHandler {
  return function (e: MouseEvent) {
    const target = e.target as HTMLElement
    if (el.contains(target)) {
      return false
    }
    if (isIgnoresElement(binding.arg, target)) {
      return false
    }
    binding.value(e)
  }
}

const clickOutsideHandle = (e: MouseEvent) => {
  const { documentHandler } = nodeList
  if (documentHandler) {
    documentHandler(e)
  }
}


export const clickOutside: ObjectDirective = {
  beforeMount(el, binding) {
    nodeList = {
      documentHandler: createDocumentHandler(el, binding)
    }
  },
  mounted() {
    window.addEventListener('click', clickOutsideHandle)
  },
  updated(el, binding) {
    nodeList = {
      documentHandler: createDocumentHandler(el, binding)
    }
  },
  unmounted() {
    window.removeEventListener('click', clickOutsideHandle)
  }
}

```

但上面的代码有一处致命的bug，一个页面中使用多次，只有最后一个元素的事件是生效的，下面将指令的实例中存储一个nodeList列表全局维护所有绑定了click-outside指令的节点，所有节点用通过一个click事件来处理，如果有存在任何一个节点，就不移除事件。支持多处使用。

```ts
// 导入Vue的指令相关类型
import type { DirectiveBinding, Directive } from 'vue'

// 定义处理文档事件的类型
type DocumentHandler = <T extends MouseEvent>(e: T) => void
// 定义列表属性接口
interface ListProps {
  el: HTMLElement
  documentHandler?: DocumentHandler
}

// 初始化节点列表
const nodeList: ListProps[] = []

// 定义选项接口
interface IOptions {
  ignores?: string | string[]
}
// 检查是否为忽略的元素
const isIgnoresElement = (options: IOptions | undefined = {}, target: HTMLElement) => {
  // 如果选项不存在或目标元素不存在，则返回false
  if (!options || !target) {
    return false
  }
  // 解构忽略列表
  const { ignores = [] } = options
  // 根据忽略列表类型进行判断
  if (typeof ignores === 'string') {
    return document.querySelector(ignores)?.contains(target)
  } else if (Array.isArray(ignores) && ignores.length > 0) {
    return ignores.every((element) => {
      return document.querySelector(element)?.contains(target)
    })
  }
  return false
}

/**
 * 创建文档事件处理函数
 *
 * @param el HTMLElement
 * @param binding binding
 * @returns Function
 */
function createDocumentHandler(el: HTMLElement, binding: DirectiveBinding): DocumentHandler {
  return function (e: MouseEvent) {
    const target = e.target as HTMLElement
    // 如果点击在目标元素内部，则返回false
    if (el.contains(target)) {
      return false
    }
    // 如果是忽略的元素，则返回false
    if (isIgnoresElement(binding.arg as IOptions, target)) {
      return false
    }
    // 执行绑定的值
    binding.value(e)
  }
}

// 点击外部处理函数
const clickOutsideHandle = (e: MouseEvent) => {
  // 遍历节点列表，执行文档处理函数
  nodeList.forEach((item) => {
    const { documentHandler } = item
    if (documentHandler && typeof documentHandler === 'function') {
      documentHandler(e)
    }
  })
}

// 点击外部指令
const clickOutside: Directive = {
  // 指令挂载时
  mounted(el, binding) {
    // 查找节点在节点列表中的索引
    const index = nodeList.findIndex((item) => {
      return item.el === el
    })
    // 如果节点不在列表中，则添加到节点列表中
    if (index === -1) {
      nodeList.push({
        el: el,
        documentHandler: createDocumentHandler(el, binding)
      })
      // 如果节点列表长度为1，则添加点击外部事件监听
      if (nodeList.length === 1) {
        window.addEventListener('click', clickOutsideHandle)
      }
    }
  },
  // 指令卸载前
  beforeUnmount(el, binding) {
    // 查找节点在节点列表中的索引
    const elInNodeListIndex = nodeList.findIndex((item) => {
      return item.el === el
    })
    // 如果节点在列表中，则从列表中移除
    if (elInNodeListIndex !== -1) {
      nodeList.splice(elInNodeListIndex, 1)
      // 如果节点列表为空，则移除点击外部事件监听
      if (nodeList.length === 0) {
        window.removeEventListener('click', clickOutsideHandle)
      }
    }
  }
}

// 导出指令名称和指令对象
export const name = 'ClickOutside'
export default clickOutside

```

## 使用方法

```html
<template>
  <!-- 有参数 -->
  <div v-click-outside:[clickOptions]="closeHandle">……</div>
  <!-- 无参数 -->
  <div v-click-outside="closeHandle">……</div>
</template>

<script setup lang="ts">
const clickOptions = {
    ignores: ['.ignore-click-out-element', '#xxxxx']
}
const closeHandle = () => {
  // 处理关闭事件
}
</script>
```

这是一个使用 Vue.js 的 `v-click-outside` 指令的示例代码。它定义了一个点击外部事件处理函数，并根据是否传入参数来决定如何使用指令。
在模板中，有两种使用 `v-click-outside` 指令的方式：

1. **带参数的用法**：

```html
<!-- 有参数 -->
<div v-click-outside:[clickOptions]="closeHandle">...</div>
```

这种方式会将 `clickOptions` 对象作为参数传递给指令的处理函数。在示例中，`clickOptions` 对象包含了一个 `ignores` 属性，用于指定要忽略的元素类名或 ID。

2. **无参数的用法**：

```html
<!-- 无参数 -->
<div v-click-outside="closeHandle">...</div>
```

这种方式直接将处理函数 `closeHandle` 作为参数传递给指令。

在脚本部分，定义了 `clickOptions` 对象和 `closeHandle` 函数。`clickOptions` 对象用于配置点击外部事件的一些选项，例如要忽略的元素。`closeHandle` 函数是点击外部事件的处理函数，用于执行关闭操作或其他逻辑。

## 处理`v-html`中的`a`标签

下面这段代码模拟从后台获取html渲染到前端，我们假定内容是安全的，html中进行了初步的数据绑定。

在线预览 [Vue SFC Playground (vuejs.org)](https://play.vuejs.org/#eNrtVt1vG0UQ/1e2x4MdKT4nOELF2BZQLFEekijJC8pF6Hw3ts85351u166jKFKLSAlqPio+qrSigkKLKtSWAA9JaQh/DLmL/cS/wOyu7yMfRFURSJV6uofbmdmZ3878ZvaWlXc8T+12QCkqJWr4lscIBdbxiK07jbKmMKopFc0xXIcyMvfhdJWUybLmEFKdnKvOTM9cnq0WSQYcBr7nWxQyo1w5XZ2ZnZpEhQc+dR0uXNGcyE2Tte056DF0Nc+t83kSrD8drG4EazuH+9cO9+5Y5l/7600f6sHn60z3G8AO9zaOvnwYXL+Niv4ftwefrgdbP4bbP6EwvPugf+9h/+CgRQfbz8LH3x3+dl0ffHEQbj44uvPJ4Nl2/8n9/u6v/d8f8WCZkk5Mnek5tuQBHjCBrilSAZaJ8nF8Iomjt7lpr9cLVh8FW3so5+BQ1GTMo8V8/go+ak23zI5quG3US9Ro8VENM7mISYx3l/J6RabpebEUCoUTUGq12r+AEu9OoKRrwHO6tmuZZ0GUBY3QeALe6/icwHe0eSP85ia+L4Qv3n0MX/jLvfDrz14Y5QmE4d3NCGEl/o7iLaSpOst8ZGpEWrXlWk42kxlJ+CyjXbItY/F93TFtQPMsj4WdNELKFdkw0naZIJpRwkGQFbQbmkVH/PZpuPEk2Po4/GonWLsf3nqMFA9v7Pe/X+3vItd/RvajJLj5gyT0n1evhbd2gv2rUQDXBtV2G9k4CMLEzpOxE3o9L1ak33+PNQ6SxuqD5/rsBE7opjDiQvV86OKp3oO63rFZFh0k2CWtBHA0TWuG5xAqaZVoF2GJoniq1gKDqXwVZ4cbWXWSvcClquUYdscEms1wzmVGRiQqgshZx0duEIKHidxymySD83IPskx6lNpyWQxYNZmssc8zK3cM2AoBm8IZ3uQojj2d4upxL6ICpby8CXDu44JB27N1BrgipGRaXWLYOqXYQia0XXE7cMdC083xNhHNLvoGe+5tg8dC0amKojLyJHViMzos5dGXDDf8KuVTKEoXcjlSomwJSTG8pWygeE8RargemAK2ysHJQxdNAC+rpmLE2dCjD0IY9nbOBMP1dWa5TpE4rgNvSa24u4T9fDSVF4qOy7LxspzJLIwUm24XfDGuYlNsxLQpH0WJaRLdcG3XL5LXxi++MWEa/xBWDLf0Xbtw2kHhzYvj9YlzHQzv5P918/Dcp3eNjY0lW1IE5NWtkFyO11sujtU0Xc1XxXwpiqk5yij+TuI8rFsNtYWB8Z9TuNAU/BHwLBv8KY+XC1u5GDnXFN223SsfCBnzOzCsCO5pgrF4hrxFe1ymKdM+UPC7OGdi3XDaC3V1dhJZklK2XbPDp9I5yhnAW6vDMUqzdzt8kOGYi+0E2sttTk7LaczRao+BQ6NDcaAiM8JeU/DH+9I5R0/gFtSJKKPKyt/vsT1M)

```html
<script setup lang="ts">
const TYPE = {
  ENTERPRISE: 'enterprise',
  PERSON: 'person',
}

const htmlText = [
  // 匹配到企业id，href和target不用写，这里只是用来表达js阻止了a链接的默认跳转
  '<a data-type="enterprise" data-eid="1111" data-name="xxx公司" href="https://wwww.baidu.com" target="_blank">xxx公司</a>',
  '<a data-type="enterprise" data-eid="333" data-name="bbb公司" href="https://wwww.baidu.com" target="_blank">bbb公司</a>',
  // 匹配到用户id
  '<a data-type="person" data-pid="2222" data-name="王某某" href="https://wwww.baidu.com" target="_blank">王某某</a>',
  // 没有匹配到用户id
  '<a data-type="person" data-pid="" data-name="李某某">李某某</a>',
]
const htmlStr = htmlText.join('')

const personClickHandle = (dataset) => {
  const { pid, name } = dataset
  // 根据参数判断是拼装路由，是否跳转、新开
  console.log(pid, name)
}
const enterpriseClickHandle = (dataset) => {
  const { eid, name } = dataset
  // 根据参数判断是拼装路由，是否跳转、新开
  console.log(eid, name)
}
const reportClickHandle = (evt) => {
  evt.preventDefault()
  const { target } = evt
  const { dataset } = target
  const keys = Object.keys(dataset)
  if (!keys.includes('type')) {
    return
  }
  const type = dataset['type']
  if (type === TYPE.ENTERPRISE) {
    enterpriseClickHandle(dataset)
  } else if (type === TYPE.PERSON) {
    personClickHandle(dataset)
  }
}
</script>

<template>
  <div class="demo">
    <div v-html="htmlStr" @click="reportClickHandle" class="report-html"></div>
  </div>
</template>

<style>
.demo {
  .report-html {
    a {
      text-decoration: none;
    }

    a[data-eid]:not([data-eid='']):hover,
    a[data-pid]:not([data-pid='']):hover {
      color: #1864dc;
    }

    a[data-type='enterprise'] {
      color: #3981f4;
    }

    a[data-type='person'] {
      color: #3981f4;
    }

    a[data-type='person'] {
      color: #3981f4;
    }

    a[data-pid=''] {
      color: #000;
    }
  }
}
</style>
```

上面这段代码中有`<a data-type="enterprise" data-eid="1111" data-name="xxx公司" href="https://wwww.baidu.com" target="_blank">xxx公司</a>`，渲染到`v-html`中点击时，会打开一个新的页面，但显然我不想让它能打开新的页面，我需要自己去控制逻辑，比如在app的webview中，我需要调用原生的方法来打开一个新的webview，好控制导航栏，如果直接a链接，会打开一个导航不受控制的webview。
所以我需要用一个事件委托来处理。

我在如容器绑定了一个点击事件，通过事件冒泡的原理来处理`a`标签的点击事件。使用`evt.preventDefault()`阻止a标签的默认行为。

接下来就可以获取元素的`data-*`，根据绑定的数据来进行相应的处理。
