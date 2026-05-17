---
layout: post
title: "深入理解 TypeScript 的模板字面量类型 - 从 ms 库的类型定义说起"
date: 2025-07-20
tags: ["前端", "TypeScript", "每天一个知识点", "React.js", "JavaScript", "代码规范"]
---
前几天在写一个 NestJS 项目时，涉及读取 token 的过期时间配置，遇到了非常经典的时间格式处理库 [ms](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fvercel%2Fms)。我使用的是它的最新测试版本 `3.0.0-canary.1`，顺便看了它的 TypeScript 类型定义，发现它用到了一个非常强大且优雅的 TypeScript 高级特性——模板字面量类型（Template Literal Types）。

这让我想深入聊聊 TypeScript 的模板字面量类型是什么，它能做什么，以及 ms 库是如何使用它来实现灵活且严谨的类型定义的。

## 什么是模板字面量类型？

TypeScript 在 4.1 版本中引入了模板字面量类型，灵感来源于 JavaScript 的模板字符串（Template Literals），但它发生在类型系统层面，用于构造新的字符串字面量类型。

简单来说，模板字面量类型就是用类似字符串模板的方式，拼接和组合字符串字面量类型，生成新的字符串字面量类型。

举个简单例子：

```typescript
type EventName = "click" | "scroll";
type PrefixedEvent = `on${Capitalize}`;

// PrefixedEvent 的类型等价于 "onClick" | "onScroll"
```

这允许我们用类型安全的方式构造复杂的字符串类型，避免“魔法字符串”导致的错误。

## ms 库中的模板字面量类型示例

ms 是一个用于解析和格式化时间字符串的库，比如 `"1s"`、`"2m"`、`"3h"` 等。它的类型定义里，用模板字面量类型定义了可接受的字符串格式：[传送门](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FDefinitelyTyped%2FDefinitelyTyped%2Fblob%2Fmaster%2Ftypes%2Fms%2Findex.d.ts)

```typescript
type Unit =
  | "Years" | "Year" | "Yrs" | "Yr" | "Y"
  | "Weeks" | "Week" | "W"
  | "Days" | "Day" | "D"
  | "Hours" | "Hour" | "Hrs" | "Hr" | "H"
  | "Minutes" | "Minute" | "Mins" | "Min" | "M"
  | "Seconds" | "Second" | "Secs" | "Sec" | "s"
  | "Milliseconds" | "Millisecond" | "Msecs" | "Msec" | "Ms";

type UnitAnyCase = Unit | Uppercase | Lowercase;

type StringValue =
  | `${number}`
  | `${number}${UnitAnyCase}`
  | `${number} ${UnitAnyCase}`;
```

这段定义的意思是：

- 可以接受纯数字字符串，如 <code>"1000"</code>- 也可以接受数字后直接跟时间单位的字符串，如 <code>"1s"</code>、<code>"3hours"</code>、<code>"5Msec"</code>- 还可以接受数字和单位中间有空格的格式，如 <code>"2 days"</code>、<code>"10 Minutes"</code>
这极大提升了类型的准确性和友好度，避免传入不合法的时间单位字符串。

## 这有什么好处？

- 类型安全：传入的字符串必须符合预定义的格式和单位集合，编译时就能捕获错误。- 代码提示：IDE 会自动提示所有有效的单位选项，减少输入错误。- 灵活强大：可以组合各种数字和单位形式，满足不同的使用习惯。
## 模板字面量类型的更多用法

模板字面量类型不仅可以拼接字符串，也能结合条件类型和映射类型，实现更复杂的类型逻辑。例如：

- 动态生成带前缀或后缀的字符串类型- 根据枚举生成 API 路由字符串- 约束格式化的字符串类型（如时间、日期、货币等）
## 总结

TypeScript 的模板字面量类型为类型系统带来了非常强大的字符串类型操作能力，像 ms 这样的小工具库通过它让类型定义既严谨又灵活，极大提升了开发体验。

如果你还没用过模板字面量类型，不妨试试看，写一些实用的字符串类型，能让你的代码更安全，自动化程度更高。

** ms 库类型定义简化示例**

```typescript
type Unit = "s" | "m" | "h" | "d" | "y";
type StringValue = `${number}` | `${number}${Unit}` | `${number} ${Unit}`;

declare function ms(value: StringValue | number, options?: { long: boolean }): string | number;
```

用好它，你的时间字符串处理将更优雅！

## 练习

[在ts的演练场练习一下](https://link.juejin.cn?target=https%3A%2F%2Fwww.typescriptlang.org%2Fplay%2F%3F%23code%2FC4TwDgpgBAqgdgS2FAvFARAZ3VAPhgWx33QAtiMATC9EdAbgChRIoBlYAJwTgHMA1AIYAbAK7Q0AAwAkAbziiCAIwicAvpLxQZ8xSvVz4SDVp0LlqtVEOJgGpo0oQAxsMGdoAM1FxnwBAD2cFAEmAAUAG4i4gBc7Fw8AtHQ%2BOb6ADRQAWD%2BQZgA-HGyUMJBvHFKAQHCEILBagCUcZgJfFppqg7OecgQAB5gCB6UcRzcfEJiElAA5ACMc5QzDqFh-YPDDYzbAPQ7UIBICYBK%2BoCG5oBvcoDePoDR6oAQ-wBKLqKcmAgR0ADCQZRIgXAiUAAVcAQTCASH%2BoIAQ80ABAmAB1NAOragBUAwAQKtdAFj-gDm5KBzKCABiUoAA5ZjAqAAIVECGElDudV4EAAPIwoASoP1gBA4JRMFAOpx0oyoA9MKJhL0%2BmyOVyeQBtAC6qCgsr5AD55YLhcApTManxgKQZnLWezOQT%2BfkBSD1VKeTL%2BXEyRSqTT6fjMlKAHQetUizJejVa9m8XX6mVK3b7QAqSoATNKgAAYoIB0-Tx2IAfiaWNBqXwIAAxTgBAhzAEBOn4llio2SvSqFVoACifVcoicdPtlMztJLSsyMdDjD2UEA%2BK6ABCNAEGaGDmYBoACZJ1oPW6oIAQtzhgDJvQBMcnDAEbpgHnE65E1gABSMwBLZfFxp5Ne0cnbObzBaLnbUYEkYagMMAI37XG6AYUVAAS%2BgBcchg%2BKztcoL7tAR62AAgpwnCCCAp6GhK3JVpwV5QUgnaym%2BgCg5FAgBcnoA0fKooAgoqAB3R46zni6AAJy0ZOEFQAAsiAB6wfBIDyphwAcQhdL0b23RwC0UBRMICAjCxbF8VxaCahOMyZDMU4AMxgEpsy0QAHBpcr9vhgDG1oAdh59vswmieJklzHErHsXBCHygpGnKWpLlaQx%2BpQP2UCAJipgD30UAA)

```bash
https://www.typescriptlang.org/play/?#code/C4TwDgpgBAqgdgS2FAvFARAZ3VAPhgWx33QAtiMATC9EdAbgChRIoBlYAJwTgHMA1AIYAbAK7Q0AAwAkAbziiCAIwicAvpLxQZ8xSvVz4SDVp0LlqtVEOJgGpo0oQAxsMGdoAM1FxnwBAD2cFAEmAAUAG4i4gBc7Fw8AtHQ+Ob6ADRQAWD+QZgA-HGyUMJBvHFKAQHCEILBagCUcZgJfFppqg7OecgQAB5gCB6UcRzcfEJiElAA5ACMc5QzDqFh-YPDDYzbAPQ7UIBICYBK+oCG5oBvcoDePoDR6oAQ-wBKLqKcmAgR0ADCQZRIgXAiUAAVcAQTCASH+oIAQ80ABAmAB1NAOragBUAwAQKtdAFj-gDm5KBzKCABiUoAA5ZjAqAAIVECGElDudV4EAAPIwoASoP1gBA4JRMFAOpx0oyoA9MKJhL0+myOVyeQBtAC6qCgsr5AD55YLhcApTManxgKQZnLWezOQT+fkBSD1VKeTL+XEyRSqTT6fjMlKAHQetUizJejVa9m8XX6mVK3b7QAqSoATNKgAAYoIB0-Tx2IAfiaWNBqXwIAAxTgBAhzAEBOn4llio2SvSqFVoACifVcoicdPtlMztJLSsyMdDjD2UEA+K6ABCNAEGaGDmYBoACZJ1oPW6oIAQtzhgDJvQBMcnDAEbpgHnE65E1gABSMwBLZfFxp5Ne0cnbObzBaLnbUYEkYagMMAI37XG6AYUVAAS+gBcchg+KztcoL7tAR62AAgpwnCCCAp6GhK3JVpwV5QUgnaym+gCg5FAgBcnoA0fKooAgoqAB3R46zni6AAJy0ZOEFQAAsiAB6wfBIDyphwAcQhdL0b23RwC0UBRMICAjCxbF8VxaCahOMyZDMU4AMxgEpsy0QAHBpcr9vhgDG1oAdh59vswmieJklzHErHsXBCHygpGnKWpLlaQx+pQP2UCAJipgD30UAA
```
