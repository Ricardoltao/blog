---
title: Set和Map
date: 2020-03-25 11:09:14
tags: Set和Map
categories: ES6
photos: ./images/jian.jpg
---

# Set 和 Map 数据结构

## Set

Set 类似于数组，但是成员的值都是唯一的，没有重复的值。

### Set 实例的属性和方法

Set 结构的实例有以下属性

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
- `Set.prototype.size`：返回`Set`实例的成员总数。

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。

- `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。
- `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `Set.prototype.clear()`：清除所有成员，没有返回值。

### 遍历操作

Set 结构的实例有四个遍历方法，可以用于遍历成员。

- `Set.prototype.keys()`：返回键名的遍历器
- `Set.prototype.values()`：返回键值的遍历器
- `Set.prototype.entries()`：返回键值对的遍历器
- `Set.prototype.forEach()`：使用回调函数遍历每个成员



## WeakSet

WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。

+ WeakSet 的成员只能是对象，而不能是其他类型的值。
+ WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。



## Map

Map 数据结构，类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

### Map 实例的属性和方法

Map 结构的实例有以下属性和操作方法：

+ size
+ Map.prototype.set(k,v)：`set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。
+ Map.prototype.get(key)
+ Map.prototype.has(key)
+ Map.prototype.delete(key)
+ Map.prototype.clear()

### 遍历方法

Map 结构原生提供三个遍历器生成函数和一个遍历方法。

- `Map.prototype.keys()`：返回键名的遍历器。
- `Map.prototype.values()`：返回键值的遍历器。
- `Map.prototype.entries()`：返回所有成员的遍历器。
- `Map.prototype.forEach()`：遍历 Map 的所有成员。



## WeakMap

`WeakMap`结构与`Map`结构类似，也是用于生成键值对的集合。

`WeakMap`与`Map`的区别有两点：

+ `WeakMap`只接受对象作为键名（`null`除外），不接受其他类型的值作为键名
+ `WeakMap`的键名所指向的对象，不计入垃圾回收机制。一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。


参考：[阮一峰--Set和Map](https://es6.ruanyifeng.com/#docs/set-map)
