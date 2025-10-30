---
title: this
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-13 11:00:00
---
# 13：this

# 一、this是哪里
+ `this`在英文中的含义是【这】。那么【这】是【哪】？
+ `this`关键字一般存在于函数中，表示一个指针，指向了**当前函数的执行对象**。也叫当前函数的**【执行】上下文。**
+ 注意**【执行】**这两个字，如果函数没有执行，那么`this`是没有内容的，只有当函数执行时，`this`才被绑定了内容。
+ 总结一句话：**谁【执行】了**`**this**`**所在的函数，**`**this**`**就是谁。**

![1680542236525-f0618a5b-7d17-40da-8fed-7222c51f4aa9.png](./img/HqQqoxdXaIBKULG-/1680542236525-f0618a5b-7d17-40da-8fed-7222c51f4aa9-324262.png)

# 二、常见的this指向
### 2.1 默认绑定
+ 当一个没有明确隶属对象的函数，被直接调用时。该函数内部的`this`指向window。

```javascript
function fn(){
  console.log(this.a);
}
var a = 10;
fn();    //10
// 这里的this指全局对象window
```

+ 当然要注意,在ES5的严格模式下，没有明确隶属对象的函数在默认执行时，其内部的`this`指向`undefined`。

### 2.2 隐式绑定
+ 所谓隐式绑定，就是将没有明确隶属对象的函数，归属到某个对象，通过该对象执行函数。  
此时函数内部的this指向该对象。

```javascript
function fn(){
    console.log(this.a);
}

var obj = {
    a:10,
    fn:fn
}
obj.fn();    // 10
// 这里的this指obj
```

+ 隐式绑定会遇到隐式丢失的情况：
    1. 当对象的方法被变量引用时，如果该变量没有从属对象，通过该变量执行函数，那么this会丢失，捕获到window。
    2. 当对象的方法，作为回调函数，传入另一个函数内执行时，this会丢失，捕获到window。

```javascript
function fn(){
    console.log(this.a);
}
var a = 20;

var obj = {
    a:10,
    fn:fn
}
obj.fn();    // 10

// 隐式丢失：虽然 f 是 obj.fn 的引用，但是 f 的执行，并没有归属对象
var f = obj.fn;
f();    // 20
setTimeout(obj.fn, 100);    // 20
```

    - 隐式丢失是可以被修复的，这就要使用下一种绑定方式：显示绑定

### 2.3 显示绑定
+ 所谓显示绑定，就是使用函数的方法，如：`call`，`apply`，`bind`等，可以强制改变this的指向。
+ 以`call`方法举例：

```javascript
function fn(){
    console.log(this.a);
}
var a = 20;

var obj = {
    a:10
};

fn.call(obj);    // 10
// 这里的this指obj
```

+ 可以利用显示绑定的方式，修复隐式丢失问题：

```javascript
function fn(){
    console.log(this.a);
}
var a = 20;

var obj = {
    a:10
};

// 隐式丢失被解决
fn.call(obj);    // 10

setTimeout(fn.bind(obj), 100);    // 10
```

+ 注意：通过修复隐式绑定，我们发现，显示绑定 的优先级要高于 隐式绑定。

### 2.4 构造函数绑定
+ 构造函数绑定，又叫`new`绑定，主要用于面向对象编程。
+ 这里还需要掌握`new`关键字的原理：
    1. 创建一个新对象
    2. 将函数中的`this`指向这个新对象
    3. 将这个新对象的`__proto__`指向函数的`prototype`
    4. 检查函数中是否主动返回对象，如果没有，则返回前三步处理好的对象

```javascript
function fn(){
    this.a = 10;
}

var f = new fn();

console.log(f.a);    // 10
// 这里的this指创建出来的对象f
```

+ 其实，只需要记住，凡是被`new`执行的函数，默认情况下，其内部的`this`都被`new`强行指向`new`出来的对象，也叫实例。

# 三、函数的方法
1. 语法：`fn.方法名()`
2. 有哪些
    - 共同点：使用第一个参数，修改`this`的指向
    - `call`：可以支持从第二个参数向后，无数个参数，都会传入原函数作为参数
        * 返回值以原函数的返回值为主
    - `apply`：只支持两个参数，且第二个参数必须为数组，数组会自动解析，将解析出的数据，都会传入原函数作为参数
        * 返回值以原函数的返回值为主
    - `bind`：可以支持从第二个参数向后，无数个参数，都会传入原函数作为参数。
        * 返回值是一个改变了this指向的新函数