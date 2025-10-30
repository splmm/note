---
title: DOM
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-12 20:58:06
---
# 11：DOM

# 一、DOM介绍
1. DOM：文档对象模型，`document`，提供了网页的相关操作。
    - `document`是`window`的子对象之一，但是DOM不属于BOM
2. DOM的文档结构是由众多成份组成的一个树状关系模型，在这个关系模型中的任何一个成份都是节点，每一个节点的数据类型，都是对象
    - 元素节点
    - 属性节点
    - 文本节点
    - 注释节点
    - 根节点
+ 操作流程：选择器->属性，样式，内容，元素

# 二、节点选择器
1. 元素节点选择器：
    - `document.getElementById("id名")`
    - `document.getElementsByClassName("class名")`
    - `document.getElementsByTagName("tag名")`
    - `document.querySelector("css选择器")`
    - `document.querySelectorAll("css选择器")`
    - `父元素.children`
    - `父元素.firstElementChild`
    - `父元素.lastElementChild`
    - `子元素.parentNode`
    - `元素.previousElementSibling`
    - `元素.nextElementSibling`
    - `document.documentElement`
    - `document.head`
    - `document.body`
    - `document.forms`
    - `document.forms[0].elements`
    - 注意：选择到的元素是单个？还是多个？
2. 其他节点选择器（了解）
    - `父元素.childNodes`
    - `父元素.firstChild`
    - `父元素.firstChild`
    - `元素.previousSibling`
    - `元素.nextSibling`
    - `元素.attributes`
    - `document`
3. 节点过滤
    - `节点.nodeType`
    - `节点.nodeName`
    - `节点.nodeValue`

# 三、属性操作
1. html属性：写在标签上的属性
    - html属性语法：`<div 属性名="属性值"></div>`
    - 操作语法
        * 自定义：
            + 获取：`元素.getAttribute("属性名")`
            + 设置：`元素.setAttribute("属性名","属性值")`
            + 删除：`元素.removeAttribute("属性名")`
        * 内置：
            + 对象语法
            + `元素.xxxAttribute()`系列
2. js属性：没有写在标签上的属性
    - 内置或自定义都可以直接使用对象的操作语法，进行操作
3. 一些内置属性
    - 样式属性：`元素.style`
    - class属性：`元素.className`
    - classList属性：`元素.classList`，数组形式
        * 添加class：`元素.classList.add("class名")`
        * 删除class：`元素.classList.remove("class名")`
        * 切换class：`元素.classList.toggle("class名")`
    - 内容属性：`元素.innerHTML`
        * 可以解析标签
    - 内容属性：`元素.innerText`
        * 不可以解析标签
    - 表单控件内容：`元素.value`
        * 表单控件的值
    - HTML5新增的自定义属性：`元素.dataset.属性名`
        * 对象类型，记录了当前元素身上所有的 data- 开头的自定义属性
        * 可以使用对象语法进行操作

# 四、样式操作
1. 设置：
    - 行内样式操作
        * 语法：`元素.style.css属性名 = "css属性值"`
2. 获取：
    - 非行内样式操作
        * 正常浏览器的语法：`getComputedStyle(元素).css属性名`
        * IE低版本浏览器的语法：`元素.currentStyle.css属性名`
    - 解决兼容

```javascript
function getStyle(ele, attr){
  if( ele.currentStyle ){
    return ele.currentStyle[attr];
	}else{
    return getComputedStyle(ele)[attr];
  }
}
```

# 五、尺寸类属性的快速获取
1. `元素.clientWidth/Height`
    - cont + padding
2. `元素.offsetWidth/Height`
    - cont + padding + border
3. `元素.scrollWidth/Height`
    - cont + padding + 可滚动的尺寸
4. `元素.offsetLeft/Top`
    - 相对于包含块偏移的距离
5. `元素.scrollLeft/Top`
    - 滚走了的距离，可设置
6. 补充：`元素.offsetParent`
    - 获取元素的偏移量参考父级（包含块）
    - 一个元素 绝对定位 的时候，它是根据谁来进行定位的，那么这个元素的偏移量参考父级就是谁

# 六、标签操作
1. 创建：`var ele = document.createElement("标签名")`
    - 插入节点：`父元素.appendChild( ele );`
2. 删除：
    - `元素.remove()`
    - `父元素.removeChild(子元素)`
3. 修改：
    - `元素.outerHTML = "<span>"+ 原标签内容 +"</span>"`
4. 替换：
    - `父元素.replaceChild(新节点, 旧节点)`
5. 克隆：
    - `元素.cloneNode(布尔)`
        * 为`true`时表示克隆所有后代节点
6. 获取：选择器
7. 补充：
    - 创建文本节点：`document.createTextNode('要写的文本内容')`
    - 插入到指定节点之前：`父元素.insertBefore(新节点, 基准节点);`