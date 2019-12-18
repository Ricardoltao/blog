---
title: 深入了解JavaScript
date: 2019-11-24 19:28:00
tags: JavaScript
categories: JavaScript
top_img: https://raw.githubusercontent.com/Ricardoltao/Photo/master/shahadat.jpg
---



# JS的参数传递

## 参数到底如何传递

> ECMAScript 中所有函数的参数都是按值传递的

普通类型是按值传递，拷贝原值，引用类型是按共享传递拷贝了引用，都是拷贝值，所以都是按值传递

这个值如果是简单类型，那么就是其本身。如果是引用类型也就是对象传递就是指向这个对象的地址。故我们可以认为参数全部都是值传递，具体例子：

### 第一个例子

```javascript
var obj = {
    n: 1
};
function foo(data) {
    data = 2;
    console.log(data); //2
}
foo(obj);
console.log(obj.n) // 1
```

传递引用类型传递的是指针。

![image](https://user-images.githubusercontent.com/15126694/30241403-3fafc13e-95b5-11e7-99f5-1f092c78c48a.png)

 首先执行`var obj = {n: 1}; `，可以看作在栈的011地址中存入了一个指向`{n:1}`堆的指针*p 

![img](https://camo.githubusercontent.com/e97db40b68c6739348c68215b893e99aa6b044c5/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031372f392f392f3435333865666535373066656465313233323131653465313334336434326265)

 接下来为声明`function foo `此时会创建函数执行上下文，产生一个变量对象，其中声明了形参data，由于函数没有执行，当前值为undefined。我们记data地址为022。关于更多变量对象的知识可以参考冴羽老师的这篇[JavaScript深入之变量对象](https://github.com/mqyqingfeng/Blog/issues/5)，本文不深入研究关于AO相关，你只需要知道在声明这个函数的时候里面的形参已经被创建出来了。 

![img](https://camo.githubusercontent.com/74cfb299ca39361602d9887579815203274faa97/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031372f392f392f6339343533643630346530653536383466646234396363623362363733323733)

 执行`foo(obj) `其中会进行参数传递，其中将obj中存储的*p拷贝给处在022地址的data，那么此时它们就指向了同一个对象，如果某一个变量更改了n的值，另一个变量中n的值也会更改，因为其中保存的是指针。 

![img](https://camo.githubusercontent.com/35dc851e4ebcff7f39b5c3c0c646c49c97927473/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031372f392f392f3932313831323661303734336639646631356566333061366430366663336636)

 进入函数内部，顺序执行`data = 2;`此时002地址存储了基本类型值，则直接存储在栈中，从而与堆中的{n:1}失去了联系。从而打印`console.log(data) // 2 `，最后发现初始开辟的{n:1}对象没有过更改，故而` console.log(obj.n) // 1`仍然打印1。 



### 第二个例子

```JavaScript
var obj = {n:1};
(function(obj){
  console.log(obj.n); //1
  obj.n=3;
  var obj = {n:2};
  console.log(obj.n) //2
})(obj);
console.log(obj.n) //3
```

![img](https://camo.githubusercontent.com/d68b740a7853af8053452f3951e355813c43702c/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031372f392f31302f3663393932636235653461306239363965353436343531303631306432643032)

 声明函数，虽然同为obj变量名，但是形参obj为AO中的属性，不会与全局造成覆盖，其拥有新的地址记作022，在未执行前其值为undefined。 

 函数立即执行，此时将全局obj赋值给形参obj，我们忽略这个重复命名的问题，其实就是将011中的 指针*p拷贝了一份给了022。同时执行第一个`console.log(obj.n)`结果即为1。 

![img](https://camo.githubusercontent.com/14e00a0a7f9ab9a3ebf2a9e34cc67076052cee9e/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031372f392f31302f3762616537666632636134393132656530356139663139383762653434656136)

 执行`obj.n=3`，此时为函数的形参即022中的obj来改变了对象内n的值。 

![img](https://camo.githubusercontent.com/2e191b1b44a608063fc851841c053aeb42b5b101/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031372f392f31302f3364646364386139666562346364336636663030393033613766656332376264)

**最关键的一步**：`var obj = {n:2}; `由于对象命名的关系可能很多童鞋就会有点懵，但依然按照同样的方式来分析即可，由于使用了var那么就是新声明一个对象，从而会在栈中压入新的地址记作033，其中存入了新的指针指向了新的对象{n:2}。从而之后打印的`console.log(obj.n)`结果则应是新开辟的对象中的n的值。

最后打印` console.log(obj.n) //3`很显然，全局的对象有过一次更改其值为3。

----



# 作用域

作用域是指程序源代码中定义变量的区域。

作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

## 静态作用域与动态作用域

JavaScript 采用的词法作用域，也就是静态作用域，函数的作用域在函数定义的时候就决定了。

而词法作用域相对的是动态作用域，函数的作用域是在函数调用的时候才决定。

例子：

```javascript
var value = 1;

function foo() {
    console.log(value);
}

function bar() {
    var value = 2;
    foo();
}

bar();

// 结果是 1

```

 主要区别：**词法作用域是在写代码或者定义时确定的，而动态作用域是在运行时确定的（this也是！）。词法作用域关注函数在何处声明，而动态作用域关注函数从何处调用**。 



----







# Js执行上下文栈

>  https://github.com/mqyqingfeng/Blog/issues/4 

## 可执行代码

JavaScript 的可执行代码的类型有三种：

+ 全局代码
+ 函数代码
+ eval代码

 举个例子，当执行到一个函数的时候，就会进行准备工作，这里的“准备工作”，让我们用个更专业一点的说法，就叫做"执行上下文(execution context)"。 

当 JavaScript 代码执行一段可执行代码时，会创建对应的执行上下文。

## 执行上下文栈

我们可以把函数的执行过程看做一个执行上下文栈，当函数被创建时就会将函数推入执行上下文栈中，当函数执行完毕后，就将该函数从栈中弹出。

----



# 变量对象

每个执行上下文，都有三个重要属性：

+ 变量对象
+ 作用域链
+ this

变量对象是与执行上下文相关的数据作用域，存储了上下文定义的变量和函数声明。

不同上下文的变量对象有所不同，可分为 **全局上下文** 下的变量对象和 **函数上下文** 下的变量对象

## 全局上下文

全局对象：是预定义对象，在顶层 JavaScript 中，可以通过 this 引用全局变量，全局对象时作用域链的头，意味着在顶层 JavaScript 中声明的所有变量都可以成为全局对象的属性。

1.可以通过 this 引用，在客户端 JavaScript 中，全局对象就是 Window 对象。

```js
console.log(this);
```

2.全局对象是由 Object 构造函数实例化的一个对象。

```js
console.log(this instanceof Object);
```

3.预定义了一堆，嗯，一大堆函数和属性。

```js
// 都能生效
console.log(Math.random());
console.log(this.Math.random());
```

4.作为全局变量的宿主。

```javascript
var a = 1;
console.log(this.a);
```

5.客户端 JavaScript 中，全局对象有 window 属性指向自身。

```javascript
var a = 1;
console.log(window.a);

this.window.b = 2;
console.log(this.b);
```

全局上下文中的变量对象就是全局对象！

## 函数上下文

 在函数上下文中，我们活动对象（activation object，AO）来表示变量对象。

活动对象不可以在 JavaScript 环境中访问，只有当进入一个执行上下文中，这个执行上下文的变量对象才会被激活。而只有被激活的变量对象，也就是活动对象上的属性才能被访问。

活动对象是进入函数上下文被创建的，它通过函数的 arguments 属性初始化， arguments 属性值是 Arguments 对象。 

## 执行过程

执行上下文的代码会分成两个阶段进行处理：分析和执行，我们也可以叫做：

1. 进入执行上下文
2. 代码执行

### 进入执行上下文

当进入执行上下文时，这时候还没有执行代码，

变量对象会包括：

1. 函数的所有形参 (如果是函数上下文)
   - 由名称和对应值组成的一个变量对象的属性被创建
   - 没有实参，属性值设为 undefined
2. 函数声明
   - 由名称和对应值（函数对象(function-object)）组成一个变量对象的属性被创建
   - 如果变量对象已经存在相同名称的属性，则完全替换这个属性
3. 变量声明
   - 由名称和对应值（undefined）组成一个变量对象的属性被创建；
   - 如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性

举个例子：

```
function foo(a) {
  var b = 2;
  function c() {}
  var d = function() {};

  b = 3;

}

foo(1);

```

在进入执行上下文后，这时候的 AO 是：

```
AO = {
    arguments: {
        0: 1,
        length: 1
    },
    a: 1,
    b: undefined,
    c: reference to function c(){},
    d: undefined
}

```

### 代码执行

在代码执行阶段，会顺序执行代码，根据代码，修改变量对象的值

还是上面的例子，当代码执行完后，这时候的 AO 是：

```
AO = {
    arguments: {
        0: 1,
        length: 1
    },
    a: 1,
    b: 3,
    c: reference to function c(){},
    d: reference to FunctionExpression "d"
}

```

到这里变量对象的创建过程就介绍完了，让我们简洁的总结我们上述所说：

1. 全局上下文的变量对象初始化是全局对象
2. 函数上下文的变量对象初始化只包括 Arguments 对象
3. 在进入执行上下文时会给变量对象添加形参、函数声明、变量声明等初始的属性值
4. 在代码执行阶段，会再次修改变量对象的属性值

分析：

```javascript
console.log(foo);

function foo(){
    console.log("foo");
}

var foo = 1;

```

打印的是函数，而不是 undefined。

因为在进入函数上下文时，首先会处理函数声明，其次处理变量声明，如果变量声明名称的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性。

----



#  作用域链 

当查找变量的时候，会先从当前上下文的变量对象中进行查找，如果没有，会在父级中的执行上下文的变量对象中查找，还是没有，就一直查找上一级上下文中的变量对象。直到全局上下文中的变量对象。这种由多个执行上下文的对象构成的链就是作用域链。



用函数的创建和激活理解作用域链：

## 函数的创建 

函数作用域在函数被定义时就决定了。

这是由于函数内部有一个内部属性 `[[scope]]`，当函数创建的时候，就会保存所有父变量到其中，可以理解 `[scope]` 就是所有父变量的层级链。但是注意：`[[scope]]` 并不代表完整的作用域链。

例如：

```javascript
function foo() {
    function bar() {
        ...
    }
}

```

函数创建时，各自的[[scope]]为：

```javascript
foo.[[scope]] = [
  globalContext.VO
];

bar.[[scope]] = [
    fooContext.AO,
    globalContext.VO
];

```

##  函数激活

当函数被激活时，进入执行上下文，创建 VO/AO 后，就会将活动对象添加到作用域链的前端。

这时候执行的作用域链我们命名为 Scope：

```javascript
Scope = [AO].concat([[scope]])

```

至此作用域链创建完毕。



# 进行梳理

结合下面例子，梳理前面讲过的执行上下文栈和变量对象，总结一下函数上下文中的作用域链和变量对象的创建过程。

```javascript
var scope = "global scope";
function checkscope(){
    var scope2 = 'local scope';
    return scope2;
}
checkscope();

```

执行过程如下：

1.checkscope 函数被创建，保存作用域链到内部属性[[scope]]

```js
checkscope.[[scope]] = [
    globalContext.VO
];

```

2.执行 checkscope 函数，创建 checkscope 函数执行上下文，checkscope 函数执行上下文被压入执行上下文栈

```js
ECStack = [
    checkscopeContext,
    globalContext
];

```

3.checkscope 函数并不立刻执行，开始做准备工作，第一步：复制函数[[scope]]属性创建作用域链

```js
checkscopeContext = {
    Scope: checkscope.[[scope]],
}

```

4.第二步：用 arguments 创建活动对象，随后初始化活动对象，加入形参、函数声明、变量声明

```js
checkscopeContext = {
    AO: {
        arguments: {
            length: 0
        },
        scope2: undefined
    }，
    Scope: checkscope.[[scope]],
}

```

5.第三步：将活动对象压入 checkscope 作用域链顶端

```
checkscopeContext = {
    AO: {
        arguments: {
            length: 0
        },
        scope2: undefined
    },
    Scope: [AO, [[Scope]]]
}

```

6.准备工作做完，开始执行函数，随着函数的执行，修改 AO 的属性值

```
checkscopeContext = {
    AO: {
        arguments: {
            length: 0
        },
        scope2: 'local scope'
    },
    Scope: [AO, [[Scope]]]
}

```

7.查找到 scope2 的值，返回后函数执行完毕，函数上下文从执行上下文栈中弹出

```
ECStack = [
    globalContext
];

```

执行函数，才会创建函数的执行上下文



# this

>  https://github.com/mqyqingfeng/Blog/issues/7 



# 执行上下文

>  https://github.com/mqyqingfeng/Blog/issues/8 



# 闭包的问题

>  https://github.com/mqyqingfeng/Blog/issues/9 

个人理解：闭包是一个有权访问另一个函数里的变量及方法的函数。

MND 对闭包的定义：

> 闭包就是有权访问自由变量的函数。
>
> 自由变量是在函数中使用的，既不是函数的参数也不是函数的局部变量的变量。
>
> 闭包 = 函数 + 函数能够访问的自由变量

举一个例子：

```js
var a = 1;
function foo(){
    console.log(a)
}
foo()

```

函数 foo 可以访问变量 a，但是变量 a 既不是 foo 函数的参数，也不是局部变量，所以 a 就是自由变量。



所有的变量和方法都通过作用域保存起来了，即使外面的函数执行完毕被弹出执行上下文栈，内部函数也是可以通过作用域链找到相应的变量和方法。 