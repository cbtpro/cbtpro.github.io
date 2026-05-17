---
layout: post
title: "Javascript中的尾调用优化 TCO⭐️⭐️⭐️⭐️⭐️"
date: 2025-02-06
tags: ["前端", "C语言", "TypeScript", "后端", "Linux", "JVM"]
---
尾调用优化（Tail Call Optimization，TCO）是 JavaScript 引擎在满足特定条件时对尾调用进行优化的一种方式。它可以显著减少函数调用的栈空间，提升递归函数的执行效率，避免栈溢出。

### 什么是尾调用？

尾调用是指一个函数的最后一步是调用另一个函数，并且直接将这个调用的结果返回。例如：

```javascript
function foo(x) {
    return bar(x);  // bar(x) 是尾调用，因为它是 foo 的最后一个操作
}
```

### 尾调用优化的原理

通常，每次函数调用都会在调用栈上添加一个新的栈帧。如果递归层级太深，就可能导致栈溢出。

尾调用优化的核心在于：
如果一个函数调用是尾调用，并且符合一定条件，JavaScript 引擎会直接复用当前的栈帧，而不是创建新的栈帧。

这意味着，尾调用优化可以让递归函数以迭代方式执行，减少栈的消耗，避免溢出。

### 尾调用优化的条件

- 调用必须在函数的最后一步执行。- 返回值直接来自尾调用函数，而不是经过额外的计算。- 必须在严格模式 ('use strict') 下执行。
示例：

```javascript
'use strict';
function factorial(n, acc = 1) {
    if (n <= 1) return acc;
    return factorial(n - 1, n * acc);  // 尾调用
}
console.log(factorial(5));  // 120
```

在这个例子中，每次递归调用都会在最后一步执行，并将计算结果传递给下一次调用，因此符合尾调用优化的条件。

### 没有尾调用优化的情况

如果递归调用后还有其他操作，尾调用优化不会发生：

```scss
function sum(n) {
    return 1 + sum(n - 1);  // 非尾调用，1 + ... 会阻止尾调用优化
}
```

在这个例子中，`sum(n-1)` 之后还有加法操作，导致尾调用优化失效。

### 尾递归示例

尾递归（Tail Recursion）是尾调用的一种特殊情况，指函数直接在尾部递归调用自身。

示例：

```javascript
'use strict';
function sum(n, acc = 0) {
    if (n <= 0) return acc;
    return sum(n - 1, acc + n);  // 尾递归
}
console.log(sum(10000));  // 不会栈溢出
```

### JavaScript 引擎支持

尾调用优化是 ES6 规范的一部分，但仅部分 JavaScript 引擎支持，部分引擎（如 Chrome 的 V8）可能默认未开启尾调用优化。

### 手动优化方法（避免栈溢出）

如果环境不支持尾调用优化，可以通过循环或手动栈的方式改写递归：

```ini
function sum(n) {
    let total = 0;
    while (n > 0) {
        total += n;
        n--;
    }
    return total;
}
console.log(sum(10000));  // 手动改写的迭代版本
```

### 小结

- 尾调用优化能有效减少栈空间，避免递归栈溢出。- 符合尾调用优化的递归函数称为尾递归。- 严格模式下可以触发尾调用优化，但在实际开发中要考虑 JavaScript 引擎的具体实现情况。- 在不支持 TCO 的环境下，可以用迭代或循环重写递归函数，避免栈溢出问题。
虽然 ES6 规范定义了尾调用优化，但实际上许多 JavaScript 引擎（如 Chrome 的 V8）并未实现尾调用优化。因此，即使写出了符合尾递归优化的代码，执行时仍然可能导致栈溢出。

### 为什么会溢出？

在 JavaScript 的大多数环境中，递归调用仍然会增加调用栈，即使是尾递归形式。因此，当递归层级过深（例如 `sum(10000)`），最终仍然会因为栈空间耗尽导致 `RangeError: Maximum call stack size exceeded`。

### 解决办法

如果目标环境不支持尾递归优化，可以使用以下方法避免栈溢出：

#### 使用循环改写

将递归逻辑转换为迭代方式，避免递归调用栈：

```ini
function sum(n) {
    let total = 0;
    while (n > 0) {
        total += n;
        n--;
    }
    return total;
}
console.log(sum(10000));  // 49995000，不会栈溢出
```

#### 模拟尾递归优化（Trampoline）

可以使用 trampoline 技术将递归改为迭代执行，避免栈溢出：

```ini
function trampoline(f) {
    return function (...args) {
        let result = f(...args);
        while (typeof result === 'function') {
            result = result();
        }
        return result;
    };
}

function sum(n, acc = 0) {
    if (n <= 0) return acc;
    return () => sum(n - 1, acc + n);  // 返回函数，而不是直接调用
}

const trampolinedSum = trampoline(sum);
console.log(trampolinedSum(10000));  // 49995000，不会栈溢出
```

原理：

- 每次递归调用返回一个函数，而不是直接递归，避免立即消耗栈空间。- trampoline 在循环中执行这些返回的函数，逐步计算最终结果。
#### 使用生成器（Generator）

也可以用生成器函数将递归转换为迭代：

```ini
function* sumGenerator(n, acc = 0) {
    while (n > 0) {
        acc += n;
        n--;
        yield;
    }
    return acc;
}

function run(generator) {
    const iterator = generator();
    let result = iterator.next();
    while (!result.done) {
        result = iterator.next();
    }
    return result.value;
}

console.log(run(() => sumGenerator(10000)));  // 49995000，不会栈溢出
```

### 结论

- 即使写了尾递归代码，由于 JavaScript 引擎不支持尾调用优化，仍可能溢出。- 实际开发中，可以使用循环、trampoline 或生成器等技术来防止栈溢出。- 如果目标环境明确支持尾调用优化（如 Safari），尾递归可以直接工作，否则建议使用上述技巧。
实际上，Chrome 浏览器V8 引擎并未实现也不计划实现尾调用优化（TCO） 。

### 具体情况：

- 在早期的 V8 版本中，曾经有计划实现尾调用优化，但后来由于复杂性、安全性以及性能收益有限等原因，V8 团队决定不实现该特性。- 截至目前（2025年），V8 引擎仍不支持尾调用优化，因此即使在 ES6 严格模式下编写符合尾递归优化的代码，Chrome 也不会进行优化。
### 为什么 V8 不支持 TCO？

1. 复杂性与性能权衡：
尾调用优化需要引擎在调用栈上进行较大的改动，而实际收益有限，尤其是与更常见的循环优化相比，尾递归的使用场景较少。2. 调试体验变差：
尾调用优化可能导致调用栈信息丢失，影响调试过程。开发者在调试时会发现栈帧少了，导致难以排查问题。3. 安全性问题：
TCO 的实现可能带来安全隐患，比如攻击者利用优化机制隐藏恶意调用链。
### 哪些浏览器支持 TCO？

目前，Safari（WebKit 引擎）是主流浏览器中唯一支持尾调用优化的浏览器。

- Safari 9 及以上版本 已支持尾调用优化。
### 如何检测运行环境是否支持 TCO

通过一个简单的方法可以快速检测运行环境是否支持 TCO

```javascript
function tcoTest(n) {
    'use strict';
    if (n === 0) {
        return 'Tail call optimization is supported.';
    }
    return tcoTest(n - 1);
}

const TCO_DETECT = 'TCO_DETECT'
console.time(TCO_DECT)
try {
    console.log(tcoTest(100000));
} catch (e) {
    // console.error(e);
    console.log('Tail call optimization is not supported.');
} finally {
    console.timeEnd(TCO_DETECT);
}
```

Safari 表现

![image.png](/assets/images/c870a6775dde.jpg)

Chrome 表现

![image.png](/assets/images/fa5b3aca7c54.jpg)
Nodejs 表现

![image.png](/assets/images/35a5012be7b9.jpg)

### 如何在 Chrome 中避免栈溢出？

由于 Chrome 不支持 TCO，可以考虑：

- 使用循环改写递归。- 使用 trampoline 技术（如之前提到的示例）。- 利用生成器函数或迭代方式代替递归。
#### 使用判断方法来决定是否使用尾调用优化的写法。

feature-detect.js

```ini
function tcoTest(n) {
    'use strict';
    if (n === 0) {
        return 'Tail call optimization is supported.';
    }
    return tcoTest(n - 1);
}

/**
 * 是否支持尾调用特性 
*/
export const ifTcoSupport = (() => {
    let bool = false;
    const TCO_DETECT = 'TCO_DETECT'
    console.time(TCO_DETECT)
    try {
        console.log(tcoTest(100000));
        bool = true
    } catch (e) {
        // console.error(e);
        console.log('Tail call optimization is not supported.');
        bool = false
    } finally {
        console.timeEnd(TCO_DETECT);
    }
    return bool;
})()
console.log(ifTcoSupport);
```

#### trampoline 技术

在计算机编程中，Trampoline（蹦床）是一种技术，主要用于实现对函数的间接调用或控制流的跳转。它的应用场景包括高阶函数的实现、尾递归的优化，以及在不支持尾调用优化的环境中防止栈溢出等。

主要应用场景：

1. 尾递归优化： 在不支持尾调用优化的编程语言或运行环境中，深度递归可能导致栈溢出。通过使用 Trampoline 技术，可以将递归调用转换为迭代过程，避免栈的增长，从而防止栈溢出。这通常通过将递归调用封装为返回一个函数（即“蹦床”），由一个循环不断执行这些函数，直到得到最终结果。2. 高阶函数实现： 在某些编程语言中，特别是函数式编程语言，Trampoline 被用于实现高阶函数。通过使用 Trampoline，可以在不增加调用栈深度的情况下，连续调用多个函数。3. 跨语言或不同调用约定的函数调用： 当需要在不同编程语言之间进行函数调用，或者在具有不同调用约定的函数之间进行调用时，Trampoline 可以作为中介，协调不同的调用约定，确保函数能够正确调用。
下面是一段 trampoline 解决尾递归优化的示例代码。

```javascript
/**
 * Trampoline 函数，将尾递归函数转换成循环执行，以防止栈溢出。
 * @param {Function} fn - 需要进行 trampoline 处理的函数。
 * @returns {Function} - 处理后的函数，可以进行深度递归而不会导致栈溢出。
 */
function trampoline(fn) {
    return function(...args) {
        let result = fn(...args); // 执行传入的函数，并获取结果
        // 当结果是一个函数时，继续执行，直到结果不再是函数为止
        while (typeof result === 'function') {
            result = result();
        }
        return result; // 返回最终结果
    };
}

/**
 * 标准的斐波那契函数，使用递归实现。
 * @param {number} n - 需要计算的斐波那契数的索引。
 * @param {number} [a=0] - 斐波那契数列中的第一个初始值。
 * @param {number} [b=1] - 斐波那契数列中的第二个初始值。
 * @returns {number} - 第 n 个斐波那契数。
 */
function fibonacci1(n, a = 0, b = 1) {
    if (n === 0) return a; // 基础情况：返回 a
    if (n === 1) return b; // 基础情况：返回 b
    return fibonacci1(n - 1, b, a + b); // 递归调用
}

const input = 100000;
try {
    console.log('fibonacci1执行结果：', fibonacci1(input));
} catch (e) {
    console.log('fibonacci1执行失败'); // 由于递归深度过大，可能会导致栈溢出
}

/**
 * 修改后的斐波那契函数，适用于 trampoline 处理。
 * @param {number} n - 需要计算的斐波那契数的索引。
 * @param {number} [a=0] - 斐波那契数列中的第一个初始值。
 * @param {number} [b=1] - 斐波那契数列中的第二个初始值。
 * @returns {Function|number} - 返回一个函数以供 trampoline 处理，或返回最终结果。
 */
function fibonacci(n, a = 0, b = 1) {
    if (n === 0) return a; // 基础情况：返回 a
    if (n === 1) return b; // 基础情况：返回 b
    return () => fibonacci(n - 1, b, a + b); // 返回一个函数，推迟计算
}

// 使用 trampoline 处理后的斐波那契函数
const trampolinedFibonacci = trampoline(fibonacci);

console.log('trampolinedFibonacci执行结果：', trampolinedFibonacci(input));

/**
 * 一个简单的递归函数示例。
 * @param {number} n - 递归的深度。
 * @returns {number} - 返回 0。
 */
function test1(n) {
    'use strict';
    if (n === 0) {
        return n; // 基础情况：返回 n
    }
    return test1(n - 1); // 递归调用
}
try {
    console.log('test1执行结果：', test1(1000000));
} catch (error) {
    console.log('test1执行失败'); // 由于递归深度过大，可能会导致栈溢出
}

/**
 * 修改后的递归函数，适用于 trampoline 处理。
 * @param {number} n - 递归的深度。
 * @returns {Function|number} - 返回一个函数以供 trampoline 处理，或返回最终结果。
 */
function test(n) {
    'use strict';
    if (n === 0) {
        return n; // 基础情况：返回 n
    }
    return () => test(n - 1); // 返回一个函数，推迟计算
}

// 使用 trampoline 处理后的递归函数
const trampolinedTest = trampoline(test);
console.log('trampolinedTest执行结果：', trampolinedTest(100000));
```

### 性能对比

- Safari 使用了尾调用优化（使用 Safari 打开，Chrome 打开会调用栈溢出）
[Safari 使用尾调用和尾调用优化的性能对比。](https://link.juejin.cn?target=https%3A%2F%2Fjsperf.app%2Ffusoje%2F6)

![image.png](/assets/images/6bd09d7f7454.jpg)

- Crhome 未使用尾调用优化
[Chrome 使用尾调用和trampoline的性能对比](https://link.juejin.cn?target=https%3A%2F%2Fjsperf.app%2Fcixibi%2F2)
![image.png](/assets/images/77ab43cf0e97.jpg)

### 结论

如果你主要针对 Chrome 或 Node.js 运行环境，不要依赖尾调用优化。建议使用传统的循环或 trampoline 技术来避免栈溢出，或者判断是否支持 TCO，如果支持 TCO，则着重针对尾调用进行优化，否则不使用尾调用优化，用循环替代或者使用 trampoline 技术。

### 参考

- [www.ruanyifeng.com/blog/2015/0…](https://link.juejin.cn?target=https%3A%2F%2Fwww.ruanyifeng.com%2Fblog%2F2015%2F04%2Ftail-call.html)- [stackoverflow.com/questions/5…](https://link.juejin.cn?target=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F54719548%2Ftail-call-optimization-implementation-in-javascript-engines)- [stackoverflow.com/questions/3…](https://link.juejin.cn?target=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F3660577%2Fare-any-javascript-engines-tail-call-tco-optimized)- [stackoverflow.com/questions/3…](https://link.juejin.cn?target=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F37224520%2Fare-functions-in-javascript-tail-call-optimized)- [特性：尾调用消除（ES6）(不再执行)](https://link.juejin.cn?target=https%3A%2F%2Fchromestatus.com%2Ffeature%2F5516876633341952)- [v8.dev/blog/modern…](https://link.juejin.cn?target=https%3A%2F%2Fv8.dev%2Fblog%2Fmodern-javascript%23proper-tail-calls)- [node.green/#ES2015-opt…](https://link.juejin.cn?target=https%3A%2F%2Fnode.green%2F%23ES2015-optimisation-proper-tail-calls--tail-call-optimisation-)
