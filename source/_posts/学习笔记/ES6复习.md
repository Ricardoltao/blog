---
title: ES6复习(上)
date: 2019-12-14 16:10:14
tags: ES6,复习笔记
categories: ES6
---

# ES6复习（Promise、Iterator）

参照阮老师的 ECMAScript 标准入门

## Promise

### 概念

Promise 是一个对象，可以将异步操作以同步操作的流程表达出来，解决*回调地狱* 的层层嵌套。

其中包含三个状态：

+ pending（进行中）
+ fulfilled（已成功）
+ rejected（已失败）

`Promise` 的状态改变只有两种：pending => fulfilled，pending => rejected。其中状态一旦改变，状态就凝固了，不会再改变，会一直保持这个结果，这时就称为 resolved（已定型）。

缺点：第一是无法取消，一旦新建就会立即执行，无法中途取消。第二是不设置回调函数会抛出错误。第三是当处于 `pending` 状态时，无法知道目前到哪一个阶段。



### 用法

```javascript
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

其中函数里有两个 `resolve` 和 `reject` 函数，由 JavaScript 引擎提供。

`resolve` 的作用是将 `Promise` 的状态从 pending => fulfilled，在异步操作成功后调用，并将异步操作的结果作为参数返回出去。`reject` 的作用是将 `Promise` 的状态从 pending => rejected，在异步操作失败时调用，并将异步操作报出的错误作为参数传递出去。

`Promise`实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数。

```javascript
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

`then`方法可以接受两个回调函数作为参数。第一个回调函数是`Promise`对象的状态变为`resolved`时调用，第二个回调函数是`Promise`对象的状态变为`rejected`时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受`Promise`对象传出的值作为参数。





## Iterator（遍历器）

Iterator 是一种统一接口，为不同数据结构提供统一的访问机制。任何数据结构只要部署了 Iterator 接口，就可以完成遍历操作，即依次处理该数据结构中的所有成员。Iterator 遍历对象是一个指针对象，其中包含 next 方法，指向下一个数据结构的成员。返回包含 value 和 done 两种属性的对象， value 是当前成员的值，done 是一个布尔值，表示遍历是否结束。

JavaScript表示 “集合” 的数据结构有：

+ 数组
+ 对象
+ ES6 新增的 Set、Map



Iterator 的作用

1. 为各种数据结构提供统一的、简单的访问接口
2. 使得数据结构的成员能够按照某种次序排列
3.  ES6 创造了一种新的遍历命令 for...of 循环，Iterator 接口主要供 for...of 消费



ES6 的一些数据原生具备 Iterator 接口（比如数组），即不用任何处理就可以被 for...of 处理，因为这些数据结构原生部署了 `Symbol.iterator`，其他数据结构（主要是对象）的 Iterator 接口，都需要自己在  `Symbol.iterator` 属性上面部署，这样才会被`for...of`循环遍历。对象之所以没有默认部署 Iterator 接口，是因为对象中的数据是非线性的。

原生具备 Iterator 接口的数据结构如下：

+ Array
+ Map
+ Set
+ String
+ TypedArray
+ 函数的 arguments 对象
+ NodeList 对象

