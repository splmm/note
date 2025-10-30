---
title: 数学对象 - Math
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-12 14:58:06
---
# 08：数学对象 - Math
# 一、对象的分类
1. 宿主对象：由JavaScript运行平台提供的对象
    - `window`：寄生于浏览器，BOM，提供了浏览器相关的操作
    - `document`：寄生于文档，DOM，提供了网页相关操作
2. 内建对象：由ECMAScript定义的对象，在任何ECMAScript的环境中都可以使用
    - 本地对象：`String`，`Number`，`Boolean`，`Array`，`Object`，`Function`，`Date`，`Promise`，`XMLHttpRequest`
    - 内置对象：`Math`
3. 自定义对象：自己定义的对象
    - `var obj = {}`

# 二、介绍
1. js 给我们提供了一些操作数字的方法，他们被打包成了一个内置对象Math，Math也称为数学对象
2. 我们无需关注某些数学运算的计算方式或逻辑，只需要知道在Math对象身上，哪个方法实现了这个数学运算即可
3. Math对象的使用：Math.xxx()

# 三、Math的方法和属性
1. **Math.random()：**生成一个 0 ~ 1 之间的随机数，包含 0，不包含1，范围为：[0, 1)

```javascript
var num = Math.random();
console.log(num);  // 一个随机数
```

2. **Math.round()：**将一个小数 四舍五入 取整

```javascript
var num1 = 10.1;
console.log(Math.round(num1));  // 10

var num2 = 10.6;
console.log(Math.round(num2));  // 11
```

3. **Math.abs() ：**返回指定数字的 绝对值

```javascript
var num = -10;
console.log(math.abs(num));  // 10
```

4. **Math.ceil()：**将指定小数 向上取整

```javascript
var num = 10.1;
console.log(Math.ceil(num));  // 11

var num2 = 10.9;
console.log(Math.ceil(num2));  // 11
```

5. **Math.floor()：**将指定小数 向下取整

```javascript
var num = 10.1;
console.log(Math.floor(num));  // 10

var num2 = 10.9;
console.log(Math.floor(num2));  // 10
```

6. **Math.max()：**参数为多个数字，返回值为 最大 的数字

```javascript
console.log(Math.max(1, 2, 3, 4, 5));  // 5
```

7. **Math.min()：**参数为多个数字，返回值为 最小 的数字

```javascript
console.log(Math.min(1, 2, 3, 4, 5));  // 1
```

8. **Math.sqrt()：**计算指定数字的平方根

```javascript
var res = Math.sqrt(4)
console.log(res);		// 2
```

9. **Math.pow(底数, 指数)：**根据传入的指定底数和指数计算结果

```javascript
var res = Math.pow(2, 4)
console.log(res);		// 16
```

10. **Math.PI：**得到 π 的值，3.1415926...

```javascript
console.log(Math.PI) // 3.141592653589793
```

    - 因为计算机的计算精度问题，只能得到小数点后 15 位
    - Math.PI 不是方法，是属性，所以在使用时，不需要加小括号
11. **Math.sin()**：计算正弦，注意，参数为弧度：交互转弧度公式：`Math.PI / 180 * deg`
12. **Math.cos()**：计算余弦，注意，参数为弧度：交互转弧度公式：`Math.PI / 180 * deg`

# 四、范围随机数


# 五、进制转换
1. 什么是进制：进制就是达到指定位置时候进一位
2. 常见的进制
    - 二进制: 0  1  10  11  100  101  110  111  1000
    - 八进制: 0  1  2  3  4  5  6  7  10  11  12  13  14  15  16  17  20  21
    - 十进制: 0  1  2  3  4  5  6  7  8  9  10  11  12 ... 99  100  101
    - 十六进制: 0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f  10 ... 19 ... 1a 1b 1c 1d 1e 1f 20 21 ...
3. 十进制转换成其它进制
    - `num.toString()`方法可以在数字转成字符串的时候给出一个进制数
        * 语法： `num.toString(你要转换的进制)`
        * 返回值：转换好进制以后的数字
        * 转换好的数字是字符串类型

```javascript
var num = 100
console.log(num.toString(2)) // 1100100
console.log(num.toString(8)) // 144
console.log(num.toString(16)) // 64
```

4. 其它进制转换成十进制
    - `parseInt()`方法可以在字符串转成数字的时候把字符串当成多少进制转成十进制
        * 语法： `parseInt(要转换的字符串，当作几进制来转换)`
        * 返回值：转换后的数字 你把数字当做几进制使用, 转换成十进制
        * 结果是数字类型

```javascript
var str = 100
console.log(parseInt(str, 8)) // 64 把 100 当作一个 八进制 的数字转换成 十进制 以后得到的
console.log(parseInt(str, 16)) // 256 把 100 当作 十六进制 的数字转换成 十进制 以后得到的
console.log(parseInt(str, 2)) // 4 把 100 当作 二进制 的数字转换成 十进制 以后得到的
```