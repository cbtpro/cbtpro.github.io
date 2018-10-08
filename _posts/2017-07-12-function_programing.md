---
layout: post
title:  "偏函数编程（Partial Function）"
date:   2017-07-12 23:50:32 +0800
categories: jekyll update
---

偏函数编程（Partial Function）  
>偏函数编程不是数学里的偏函数

判断对象是不是某种类型的数据，可以写以下的函数。
```javascript
var isString = function(obj) {
    return toString.call(obj) === '[object String]';
}

var isArray = function(obj) {
    return toString.call(obj) === '[object Array]';
}

var isFunction = function(obj) {
    return toString.call(obj) === '[object Function]';
}
```
发现稍微修改下，可以节省很多代码。
```javascript
var isType = function(type) {
    return function(obj) {
        return toString.call(obj) === '[object ' + type + ']';
    };
}

var isFunction = isType('Function');
var isArray = isType('Array');
```
通过这种指定部分参数来产生一个新的制定函数的形式就是偏函数

参考资料 深入浅出Node.js 第四章 异步编程 函数式编程