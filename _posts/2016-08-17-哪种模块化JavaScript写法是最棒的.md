---
layout: post
title:  "哪种模块化JavaScript写法是最棒的"
date:   2016-08-17 23:58:28 +0800
categories: jekyll update
---


结构化写法,只是简单的将函数按功能放在不同的js文件中，算一个模块。
<pre>var sign = false;
function doSomeThing(sg) {
  //函数执行
  sg = true;
}
function main() {
  doSomeThing(sign);
}
main();</pre>
使用时，直接调用函数名称和变量名称。缺点明显，单文件模块积少成多，就会出现变量名称重复、方法暴露在外层、随意更改对象状态等等问题。

对象写法，开始有面向对象的感觉了。
<pre>var Student = {
  name: '张三',
  study: function(subject) {
     console.log(this.name + '开始学习' + subject);
  }
}
Student.study('历史');
var Teacher = {
  name: '李老师',
  teach: function(subject) {
    console.log(this.name + '开始教' + subject);
  }
}
Teacher.teach('物理');</pre>
显而易见这种写法出现了相同的属性和函数，但是互相不冲突，所有这个对象的属性都可以封装到这个对象中，但是这仍然不是最优的写法。成员的变量仍然可以在外部通过Student.name = '李狗蛋'，而被修改。

隐藏不想被公开的属性和函数
<pre>var Student = function() {
  var name = '张三';
  var study = function(subject) {
    console.log(name + '开始学习' + subject);
  }
  var truancy = function() {
    console.log(name + '逃课了');
  }
  return {
    study: study
  }
}();

Student.study('中国现代史');</pre>
这种写法隐藏了name属性，不能在外部直接更改name，同时也不允许在外部执行truancy函数，想暴露什么都可以在return里定义，起到了很好地保护作用。

异步模型定义编程模式，也是我们常说的AMD规范。这种模式是目前非常流行的编程模式，nodejs、requirejs、angularjs等都是用这种模式来编写代码。

这里以Nodejs为例

hello.js
<pre>function hello(name) {

  console.log('hello, ' + name);

}

exports.hello = hello;</pre>
只要将hello模块安装后就可以使用了。

使用方法
<pre>const http = require('http');
const hello = require('hello');
const hostname = '192.168.1.10';
const prot = 8081;
const server = http.createServer((request, response) =&gt; {
  response.statusCode = 200;
  response.setHeader('Content-Type', 'text/plain');
  response.end('Hello World');
});
server.listen(port, hostname, () =&gt; {
  console.log('Server running at http://${hostname}:${port}');
  hello.hello('Sandy');
});</pre>
可以看到在AMD规范比较灵活，包含了结构化的写法、对象的写法以及AMD的define的写法。技术在进步，各种规范层出不穷，语言的语言越来越多样化，合理的组织结构让代码更加的简洁易读，所以并不限定某种写法。