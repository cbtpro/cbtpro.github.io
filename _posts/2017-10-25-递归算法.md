---
layout: post
title:  "递归算法"
date:   2017-10-25 20:32:05 +0800
categories: jekyll update
---

# 递归算法

工作中可能会去遍历一个树状的菜单，如何去写一个比较好、效率高的递归函数来遍历菜单？

1. 首先要生成一个树的数组

```javascript
    var menus = [{
      id: 1,
      name: "系统",
      menus: [{
        id: 2,
        name: "系统管理",
        menus: [{
          id:3,
          name: "权限管理"
        }]  
      }]
    },{
      id: 3,
      name: "商品",
      menus: [{
        id: 4,
        name: "商品管理",
        menus: [{
          id:5,
          name: "商品列表"
        }]  
      }]
    },{
      id: 6,
      name: "用户",
      menus: [{
        id: 7,
        name: "用户管理",
        menus: [{
          id:8,
          name: "用户列表",
          menus: [{
            id: 9,
            name: "新增用户"
          }]
        }]  
      }]
    }];
```

上面是一个简单的树，先基于这个树来写一个递归函数。

2. 迭代菜单这个数组

```javascript
    function go(menus) {
        for(var i=0;i<menus.length;i++) {
            handler(menus[i]);
        }
    }
```

   ​

3. 处理每个树中的每一个对象

```javascript
    function handler(menu) {
        console.log('找到菜单的name是: ' + menu.name);  
        if(menu.menus) {
            go(menu.menus)
        }
    }
```


4. 调用go函数,并输出结果

```javascript
    go(menus);
    //找到菜单的name是: 系统
    //找到菜单的name是: 系统管理
    //找到菜单的name是: 权限管理
    //找到菜单的name是: 商品
    //找到菜单的name是: 商品管理
    //找到菜单的name是: 商品列表
    //找到菜单的name是: 用户
    //找到菜单的name是: 用户管理
    //找到菜单的name是: 用户列表
    //找到菜单的name是: 新增用户
```

现在要从从树中找到“商品管理”这个菜单，怎么做？

1. 还是使用上面的树

2. 迭代这个数组，并且传入商品管理的id来作为匹配条件

```javascript
    function go(menus, menuId) {
        for(var i=0;i<menus.length;i++) {
            handler(menus[i], menuId);
        }
    }
```

   ​

3. 处理每个树中的每一个对象,并且在找到菜单的时候，中断操作

```javascript
    function handler(menu, menuId) {
        console.log('遍历了菜单: ' + menu.name);
        if(menu.id === menuId) {
            console.log('找到菜单id为: ' + menuId);
            console.log(menu);
            return;
        }
        if(menu.menus) {
            go(menu.menus, menuId)
        }
    }
```

4. 调用go函数,并输出结果

```javascript
go(menus, 5);
//遍历了菜单: 系统
//遍历了菜单: 系统管理
//遍历了菜单: 权限管理
//遍历了菜单: 商品
//遍历了菜单: 商品管理
//遍历了菜单: 商品列表
//找到菜单id为: 5
//{id: 5, name: "商品列表"}
//遍历了菜单: 用户
//遍历了菜单: 用户管理
//遍历了菜单: 用户列表
//遍历了菜单: 新增用户
```



但是看控制台输出的结果，发现在找到符合的菜单后，仍然将剩下的所有菜单都遍历了一遍，这不是我们想要看到的结果。

那么怎么让找到结果后，不继续遍历剩下的节点呢？

在for循环中通过handler函数的返回值来判断找到了节点，然后跳出循环是不可行的，因为for循环已经在内存中创建了，无法取消。

但是可以通过标志位，让handler中的事件不继续执行。

只需要添加一个stop的标志就可以

```javascript
    var stopRecursionFlag; //停止遍历的标志位
    function handler(menu, menuId) {
        if(stopRecursionFlag) {
            return;
        }
   	    console.log('遍历了菜单: ' + menu.name);
   	    if(menu.id === menuId) {
            stopRecursionFlag = true;
            console.log('找到菜单id为: ' + menuId);
            console.log(menu);
            return;
        }
        if(menu.menus) {
            go(menu.menus, menuId)
        }
    }
    function go(menus, menuId) {
        for(var i=0;i<menus.length;i++) {
            handler(menus[i], menuId);
        }
    }
    go(menus, 5);
    //遍历了菜单: 系统
    //遍历了菜单: 系统管理
    //遍历了菜单: 权限管理
    //遍历了菜单: 商品
    //遍历了菜单: 商品管理
    //遍历了菜单: 商品列表
    //找到菜单id为: 5
    //{id: 5, name: "商品列表"}
```



# 挑战

接下来挑战一下，生成一个10000000个节点的菜单树，在这1w个菜单的节点中找到对应的菜单。

```javascript
function getRandomNum(min,max) {   
    var range = max - min;   
    var rand = Math.random();   
    return(min + Math.round(rand * range));   
}
function randomString(len) {
　　len = len || 32;
　　var $chars = 'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678';    /****默认去掉了容易混淆的字符oOLl,9gq,Vv,Uu,I1****/
　　var maxPos = $chars.length;
　　var str = '';
　　for (i = 0; i < len; i++) {
　　　　str += $chars.charAt(Math.floor(Math.random() * maxPos));
　　}
　　return str;
}
function start(stratum, menu) {
    var menus = [];
    while(menuId < stratum) {

        menus.push(function() {
            var menu = {
                id: menuId,
                name: randomString(10)
            };
            menuId++;
            var menus = [];
            for(var i=0;i<3;i++) {
                menus.push(function() {
                    var menu = {
                        id: menuId,
                        name: randomString(10)
                    };
                    menuId++;

                    var menus = [];
                    for(var i=0;i<3;i++) {
                        menus.push(function() {
                            var menu = {
                                id: menuId,
                                name: randomString(10)
                            };
                            menuId++;
                            return menu;
                        }());
                    }
                    menu.menus = menus;
                    return menu;
                }());
            }
            menu.menus = menus;
            return menu;
        }());
        process();

    }
    menu.menus = menus;
}
var lastPrecent;
function process() {
    var precent = Math.floor(menuId/menuNum*100);
    if(precent%2 === 0 && lastPrecent !== precent) {
        console.clear();
        lastPrecent = precent;
        console.log('当前进度: ' + precent + '%');
    }
};
var menuId = 0;
function buildMenu(menuNum, menu) {
    console.log('开始生成数据');
    var startTime1 = new Date();
    menuId = 0;
    start(menuNum, menu);
    var endTime1 = new Date();
    var menus = menu.menus;
    console.log('开始时间：' + startTime1);
    console.log('结束时间：' + endTime1);
    console.log('总耗时：' + (endTime1 - startTime1) + 'ms');
    console.log('共生成：' + menuNum + '个菜单');
    return menu.menus;
}
var stopRecursionFlag; //停止遍历的标志位
function handler(menu, menuId) {
    if(stopRecursionFlag) {
        return;
    }
    //console.log('遍历了菜单: ' + menu.name);
    if(menu.id === menuId) {
        stopRecursionFlag = true;
        console.log('找到菜单id为: ' + menuId);
        console.log(menu);
        return;
    }
    if(menu.menus) {
        go(menu.menus, menuId)
    }
}
function go(menus, menuId) {
    for(var i=0;i<menus.length;i++) {
        handler(menus[i], menuId);
    }
}
function findMenu(menus, id) {
    console.log('开始查询数据');
    var startTime = new Date();
    stopRecursionFlag = false;
    go(menus, id);
    var endTime = new Date();
    console.log('开始时间：' + startTime);
    console.log('结束时间：' + endTime);
    console.log('总耗时：' + (endTime - startTime) + 'ms');
}

var menu = {};
var menuNum = 10000000;
var menus = buildMenu(menuNum, menu);

findMenu(menus, 100);
findMenu(menus, 9999990);
```

发现查找不同位置的menu，消耗的时间不同，

下一次来优化这个查找的算法。