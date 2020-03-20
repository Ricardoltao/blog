---
title: for-in和for-of
date: 2020-03-13 13:18:10
tags:
- for-in 和 for-of
categories: JavaScript
---
# for in 和 for of

## 前言

1. 推荐在循环对象属性的时候，使用`for...in`,在遍历数组的时候的时候使用`for...of`。
2. `for...in`循环出的是key，`for...of`循环出的是value
3. 注意，`for...of`是ES6新引入的特性。修复了ES5引入的`for...in`的不足
4. `for...of`不能循环普通的对象，需要通过和`Object.keys()`搭配使用

## for in

for in 作用于数组的话，循环除了遍历数组元素以外，还会遍历自定义属性，如果你的数组中有一个可枚举的类型 a.name，那么循环将额外遍历一次，遍历名为 name 的索引，甚至数组原型上的属性都能访问到，所以不适用于数组的遍历

```js
Object.prototype.objCustom = function() {}; 
Array.prototype.arrCustom = function() {};

var arr = [3, 5, 7];
arr.foo = 'hello';

for (var i in arr) {
   console.log(i);
}
// 结果是：
// 0
// 1
// 2
// foo
// arrCustom
// objCustom
```

遍历了原型对象，在实际工作中是不需要的，多此一举

用 hasOwnProperty

```js
Object.prototype.objCustom = function() {}; 
Array.prototype.arrCustom = function() {};

var arr = [3, 5, 7];
arr.foo = 'hello';

for (var i in arr) {
   if (arr.hasOwnProperty(i)) {
    console.log(i);
  }
}
// 结果是：
// 0
// 1
// 2
// foo
```

数组本身的属性还是会遍历出来，用 forEach 

```js
Object.prototype.objCustom = function() {}; 
Array.prototype.arrCustom = function() {};

var arr = [3, 5, 7];
arr.foo = 'hello';

arr.forEach(function (value, i) {
  console.log(i);
});
// 结果是：
// 0
// 1
// 2
```

但是 forEach 只能遍历数组，而且在遍历数组的时候无法 break 或 return false 中断



## for of

for of 弥补了 for in 循环的所有缺点，一般用于数组的遍历，也支持大部分类数组对象。 

**注意:`for-of`循环不支持普通对象,但是如果你想迭代一个对象的属性,可以使用`for-in`循环(这也是它的本职工作)或者内建的`Object.keys()方法`**

```js
var student={
    name:'wujunchuan',
    age:22,
    locate:{
    country:'china',
    city:'xiamen',
    school:'XMUT'
    }
}
for(var key of Object.keys(student)){
    //使用Object.keys()方法获取对象key的数组
    console.log(key+": "+student[key]);
}
```

还不如用 for in来遍历

**for of 的其他应用场景：**

for of可以用来迭代字符串

```js
let str = 'boo';

for (let value of str) {
  console.log(value);
}
// 结果是：
// "b"
// "o"
// "o"
```

可以迭代 arguments类数组对象，迭代NodeList这类DOM集合，无需`[].slice.call()`，也不需要`Array.from()`进行数组转化

```js
let elements = document.querySelectorAll('body');

for (let element of elements) {
  console.log(element.tagName);
}
// 结果是：
// "BODY"
```

还可以迭代类型数组、Map、Set、generators

## 参考

[JavaScript中的for-of循环](https://github.com/wujunchuan/wujunchuan.github.io/issues/11)

[看，for..in和for..of在那里吵架！](https://www.zhangxinxu.com/wordpress/2018/08/for-in-es6-for-of/)
