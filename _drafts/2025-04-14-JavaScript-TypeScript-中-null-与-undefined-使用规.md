---
layout: post
title: "JavaScript / TypeScript 中 `null` 与 `undefined` 使用规范"
date: 2025-04-14
tags: ["前端", "JavaScript", "每天一个知识点", "TypeScript", "编译原理", "面试"]
---
## 一、目的

为了提升代码一致性、可读性和可维护性，统一团队中 `null` 和 `undefined` 的使用场景与规则，避免混乱与隐患。

## 二、概念区分

| 类型 | 含义 | 说明 || --- | --- | --- || undefined | 系统默认的“未赋值”状态 | 通常由 JavaScript 引擎自动赋值 || null | 明确表示“空”或“无值” | 由程序员主动赋值，表达“空”或“无效”状态 |
## 三、使用规范

### 1. 变量初始化

- 使用 null 初始化非基本类型变量（对象、数组、引用）：

ini
```javascript
let user: User | null = null;
let items: Item[] | null = null;
```
- 禁止手动将变量赋为 undefined，未初始化变量默认为 undefined 即可。
### 2. 对象属性缺省/清空

- 对象属性可初始化为 null，表示“暂时为空”：

csharp
```typescript
interface Profile {
  avatarUrl: string | null;
}
```
- 禁止将对象属性赋为 undefined，使用 null 代替。
### 3. 函数参数与返回值

- 返回值为空时建议返回 null，避免使用 undefined：

```typescript
function findUser(id: number): User | null {
  return users[id] ?? null;
}
- 函数可选参数应使用 ? 来声明，而非 | undefined：
```

php
```typescript
function greet(name?: string) {
  const realName = name ?? 'Guest';
}

### 4. 判断空值

- 判断变量是否为“空”时，使用宽松等于 == null：
```

ini
```text
if (value == null) {
  // 相当于 value === null || value === undefined
}
- 避免使用 typeof value === 'undefined'，除非在处理全局变量或 window 属性。
### 5. 清除引用（优化垃圾回收）
```

- 使用 null 显式清除引用，有助于垃圾回收：

ini
```text
largeObject = null;
- 不推荐使用 delete obj.prop，可使用 obj.prop = null。
## 四、禁止示例
```

以下写法不符合规范：

```javascript
// 不推荐
let data = undefined;
obj.value = undefined;
return undefined;

// 推荐
let data: SomeType | null = null;
obj.value = null;
return null;
```

## 五、TypeScript 类型声明建议

- 对于可能缺省的变量或返回值，使用 | null 明确标注：

csharp
```typescript
let token: string | null = null;
- 函数参数若为可选项，应使用 ? 而非 | undefined：
```

```typescript
function load(id?: number) { ... }

## 六、配套 ESLint 规则建议（可选）
```

```json
{
  "rules": {
    "no-undefined": "error", // 禁止手动使用 undefined
    "@typescript-eslint/strict-boolean-expressions": "warn",
    "@typescript-eslint/no-unnecessary-condition": "warn"
  }
}
```

## 七、总结原则

- 程序员主动赋值请使用 null。- 系统默认缺省状态使用 undefined，无需手动赋值。- 清除对象引用时使用 null，有助于垃圾回收。- 判断空值时使用 == null，可同时判断 null 和 undefined。- 类型声明中建议优先使用 | null，参数可选用 ?。
