---
title: 解决safari浏览器使用window.open失效的问题
date: 2023-07-10 04:58:06
categories: 
    - 前端
tags:
    - JavaScript
    - 浏览器兼容性

---

## 问题描述

在Safari浏览器中，由于安全机制的限制，`window.open()`在回调函数中可能会失效。这是Safari浏览器为了防止页面滥用弹窗（如广告）而实施的安全策略。

## 现象分析

正常使用时：
```javascript
window.open(url, '_blank'); // 正常工作
```

回调函数中使用时：
```javascript
axios.get('xxxxxx').then((url) => {
        window.open(url, '_blank'); // 失效
});
```

## 解决方案

### 1. 预先声明窗口对象

```javascript
let newWindow = window.open('', '_blank');
axios.get('xxxxxx').then((url) => {
        newWindow.location = url;
});
```

### 2. 使用超链接模拟点击

```javascript
axios.get('xxxxxx').then((url) => {
        let dom = document.createElement('a');
        dom.setAttribute('href', url);
        document.body.appendChild(dom);
        dom.click();
});
```

### 3. 使用window.location

```javascript
axios.get('xxxxxx').then((url) => {
        window.location.href = url;
        // 或者使用
        // window.location.assign(url);
});
```

## 总结

为解决Safari浏览器中`window.open`在回调函数中的限制，我们可以：
1. 提前创建窗口对象
2. 使用DOM元素模拟点击
3. 直接使用window.location跳转

选择合适的方案取决于具体的使用场景。