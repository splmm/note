---
title: BOM
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-12 18:58:06
---
# 10：BOM

# 一、BOM介绍
1. BOM是Browser Object Model的缩写，简称浏览器对象模型。这个对象就是`window`
2. BOM提供了独立于内容而与浏览器窗口进行交互的对象
3. BOM由一系列相关的对象构成，并且每个对象都提供了很多方法与属性
4. BOM缺乏标准，JavaScript语法的标准化组织是ECMA，DOM的标准化组织是W3C
5. 由于BOM主要用于管理窗口与窗口之间的通讯，因此其核心对象是`window`。
6. 因为js最终被运行在浏览器环境中，所以js中所有的数据最终都会被window收录
    - 具体的表现为：所有的全局，都被直接绑定在了window身上，尤其是js自带的全局
        * window.alert() => alert()
        * window.parseInt() => parseInt()
        * window.parseFloat() => parseFloat()
        * window.document.write() => document.write()
        * ...
        * window在使用过程中可以省略
    - 没有明确隶属对象的函数，也都指向window
        * 需要配合关键字：`this`，查看

```javascript
// 有隶属对象
var obj = {
    fn:function(){};
}
obj.fn();

// 全局函数，没有隶属对象
function fn(){}
fn();

function box(){
    function fn(){}
    // 局部函数，没有隶属对象
    fn();
}
box();

function box2(f){
    // 回调函数，没有隶属对象
    f();
}
box2( function(){} );
```

# 二、window的子对象
![1680193763566-66228235-bf25-4d0d-8070-2bc93eba8b88.png](./img/QdCWFF_kLvyEtH7l/1680193763566-66228235-bf25-4d0d-8070-2bc93eba8b88-129154.png)

1. `history`：历史记录
    - 方法：
        * 后退：`history.back()`
        * 前进：`history.forward()`
        * 走n步：`history.go(n)`
            + 正：前进；负：后退；0：刷新
    - 属性：
        * 个数：`history.length`
2. `location`：地址
    - 属性：
        * 完整URL：`location.href`
        * 协议：`location.protocol`
        * 域：`location.host`
        * 域名：`location.hostname`
        * 源：`location.origin`
        * 端口：`location.port`
        * 路径名：`location.pathname`
        * 查询数据：`location.search`
        * 锚点连接：`location.hash`
    - 方法：
        * 刷新：`location.reload()`
        * 跳转到指定url：`location.assign("url")`
3. `navigator`：浏览器信息
    - 浏览器信息：`navigator.userAgent`
4. `screen`：视窗大小
5. `frames`：框架对象，配合`iframe`标签使用
6. `document`：文档，网页
7. `localStorage` & `sessionStorage`：本地存储

# 三、window的方法和事件
1. window的方法
    - `alert()`
        * 信息框
    - `confirm()`
        * 选择框，注意返回值
    - `prompt()`
        * 对话框，注意返回值
    - `open()`
        * `open("url", "_blank", "width=300,height=300,left=200,top=100")`
    - `close()`
        * 注意浏览器拦截机制，部分浏览器有兼容
    - `scrollTo()`
        * 参数为两个数值或一个对象
        * `scrollTo(x, y)`
        * `scrollTo({top: y, left: x, behavior: 'smooth'})`
2. window的事件
    - `load`
        * 页面结构加载完成
        * 外部资源加载完成
    - `scroll`
        * 获取滚走了的距离
            + `document.documentElement.scrollTop`
            + `document.documentElement.scrollLeft`
    - `resize`
        * 获取当前可视区域的尺寸
            + `document.documentElement.clientWidth`
            + `document.documentElement.clientHeight`

# 四、定时器
1. 计时器
    - 每隔指定时间，执行一次指定功能
    - 开启：`setInterval(参数1, 参数2)`
        * 参数1：回调函数
        * 参数2：毫秒数
        * 每隔指定的毫秒数，执行一次回调函数
        * 返回值：当前计时器的唯一标志，用来关闭自身
            + 建议：如果这个计时器要被清除，最好将返回值保存在变量中，将来通过变量清除当前计时器
    - 关闭：`clearInterval(参数)`
        * 参数：要清除的计时器的唯一标志（返回值）
2. 延时器
    - 延迟指定时间，只执行一次指定功能
    - 开启：`setTimeout(参数1, 参数2)`
        * 参数1：回调函数
        * 参数2：毫秒数
        * 延迟指定的毫秒数，只执行一次回调函数
        * 返回值：当前延时器的唯一标志，用来关闭自身
            + 建议：如果这个延时器要被清除，最好将返回值保存在变量中，将来通过变量清除当前延时器
    - 关闭：`clearTimeout(参数)`
        * 参数：要清除的延时器的唯一标志（返回值）