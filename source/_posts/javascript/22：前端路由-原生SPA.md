---
title: 前端路由-原生SPA
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-14 12:00:00
---
# 22：前端路由-原生SPA

# 一、什么是路由
1. 路由：就是根据不同的地址，响应不同的内容。一般情况下，在项目开始之前，都会对路由地址进行规划、设定，然后在项目开发过程中，针对预先规划的路由地址进行处理和响应。
    - 就是地址和页面一一对应关系的集合
    - 就是一个 url 地址，对应哪个组件（页面）
2. 前端路由的本质
    - 根据地址栏变化(不重新向服务器发送请求)，局部更新不同的页面内容，完成前端业务场景切换
3. 前端路由的思路
    - URL 地址栏中的 Hash 值发生变化
    - 前端 JS 监听 Hash 地址的变化
        * `window.onhashchange = function(){  }`
    - 前端 JS 把当前 Hash 地址对应的组件渲染到浏览器中 

# 二、什么是SPA
1. SPA：单页面应用，Single Page Application
    - 只有Web 页面的应用，是加载单个 HTML 页面，并在用户与应用程序交互时，动态更新该页面的 Web 应用程序
2. 简单实现

```plain
<body>
    <div class="box">
        <div class="top"> 顶部通栏 </div>
        <div class="bottom">
            <div class="slide">
                <a href="#/pageA">pageA</a>
                <a href="#/pageB">pageB</a>
                <a href="#/pageC">pageC</a>
                <a href="#/pageD">pageD</a>
            </div>
            <div class="content router-view">内容区</div>
        </div>
    </div>
    <script>
        // 准备一些渲染内容, 后续会根据 Hash 值去展示
        const templateA = `<div>templateA</div>`
        const templateB = `<div>templateB</div>`
        const templateC = `<div>templateC</div>`
        const templateD = `<div>templateD</div>`
        
        // 获取 Dom 节点
        const rw = document.querySelector('.router-view')
        
        // 监听 hash 值的变化，hash值发生变化时会触发该事件
        window.onhashchange = function () {
            const { hash } = window.location;
            
            // 根据当前的 Hash 值, 决定渲染哪一段内容
            if (hash === '#/pageA') {
                rw.innerHTML = templateA
            } else if (hash === '#/pageB') {
                rw.innerHTML = templateB
            } else if (hash === '#/pageC') {
                rw.innerHTML = templateC
            } else if (hash === '#/pageD') {
                rw.innerHTML = templateD
            }
        }
    </script>
</body>
```