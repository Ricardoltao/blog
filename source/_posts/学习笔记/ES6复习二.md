---
title: ES6复习(二)
date: 2019-12-15 18:20:15
tags: 
- ES6
- 复习笔记
categories: ES6
top_img: https://raw.githubusercontent.com/Ricardoltao/Photo/master/shahadat.jpg
---

# ES6复习

## Generator

Generator 函数是 ES6 提供的一种异步解决方案，语法行为与传统的函数不同。

语法上，可以将 Generator 函数理解成一个状态机，封装了多个内部状态。执行 Generator 函数会返回一个遍历器对象，也就是说它也是一个遍历器对象生成函数。返回遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

形式上，Generator函数是一个普通的函数，有两个特征需要注意，
1. `function` 关键字与函数之间有一个星号
2. 函数内部使用 `yield` 表达式，定义不同的内部状态

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
```

其中调用 Generator 函数后，函数并不执行，返回的也不是函数运行的结果，而是一个指向内部状态的指针对象，也就是遍历器对象（Iterator Object）。
所以是对返回的遍历器对象进行操作，所以要调用遍历器对象的 `next` 方法。Generator 函数是分段执行的，`yield` 表达式是暂停执行的标记，而 `next` 方法可以恢复执行。

`for...of` 可以自动遍历 Generator 函数运行时生成的 Itearator 对象，且此时不再需要 `next` 方法。

## async

async 函数就是 Generator 函数的语法糖。
```js
const fs = require('fs');

const readFile = function (fileName) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function(error, data) {
      if (error) return reject(error);
      resolve(data);
    });
  });
};

const gen = function* () {
  const f1 = yield readFile('/etc/fstab');
  const f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

上面代码的函数gen可以写成async函数，就是下面这样。
```js
const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

一比较就会发现，async 函数就是将 Generator 函数的星号（*）替换成 async，将 yield 替换成 await，仅此而已。

具体文章还是看阮老师的 ES6标准入门，讲的比较清楚。