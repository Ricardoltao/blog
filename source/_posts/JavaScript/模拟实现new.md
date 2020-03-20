---
title: 模拟实现new
date: 2020-02-22 18:51:27
tags: new的模拟实现
categories: JavaScript
---
# 模拟实现new

## 前言

今日复习一下new操作符，new是用于创建构造函数的实例，其中创建的实例对象拥有构造函数的属性、方法和原型上的属性和方法。

## 实现

**分析**：

因为 new 的结构是一个新对象，所以在模拟实现的时候，我们也要建立一个新对象，假设这个对象叫 obj，obj会具有构造函数里的属性，根据经典继承的例子，可以使用 Constructor.apply(obj,arguments) 来给 obj 添加新的属性。

根据原型及原型链的知识，我们可以知道实例的 `__proto__` 属性会指向构造函数的 prototype，所以用这个特性来实现实例访问原型上的属性

**代码**:

```javascript
function objectFactory(){
    var obj =  new Object(), //从Object.prototype上克隆一个对象
        Constructor = [].shift.call(arguments); //取得外部传入的构造器
    obj.__proto__ = Constructor.prototype;
    var ret = Constructor.apply(obj,arguments);//借用外部传入的构造器给obj设置属性
    return typeof ret === 'object' ? ret : obj;//确保构造器总是返回一个对象
}
```



## 最后

[参考文献](https://github.com/mqyqingfeng/Blog/issues/13)