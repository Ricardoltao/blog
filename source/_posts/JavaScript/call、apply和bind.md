---
title: 模拟实现call、apply和bind
date: 2020-02-22 14:11:20
tags: 模拟实现call、apply和bind
categories: JavaScript
---

# 模拟实现call、apply和bind

## 前言

手动实现call、apply和bind是面试常考题，对这方面做一次梳理。



## call

```javascript
Function.prototype.myCall = function(context){
    var context = context || window;
    context.fn = this;//此处this是指调用myCall的function
    var args = [...arguments].slice(1);
    var result = context.fn(...args);
    delete context.fn; //将该函数删除
    return result;
}
```

## apply

```javascript
Function.protptype.myApply = function(context=window){
    context.fn = this;
    let result;
    if(arguments[1]){
        result = context.fn(...arguments[1]);
    }else{
        result = context.fn()
    }
    delete context.fn;
    return result;
}
```

## bind

```javascript
Function.prototype.myBind = function(context){
    if(typeof this != 'function'){
        throw Error('not a function');
    }
    let self = this;
    let args = [...arguments].slice(1);
    let resFn = function(){
        return self.apply(this instanceof resFn?this:context,args.concat(...arguments));
    }
    return resFn;
}
```

