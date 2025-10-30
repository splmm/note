---
title: Node.js
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-15 16:00:00
---
# 29：Node.js

# 一、web服务构成
1. 服务器： 
    - 也称伺服器，是提供计算服务的设备。由于服务器需要响应服务请求，并进行处理，因此一般来说服务器应具备承担服务并且保障服务的能力。
    - 在网络环境下，根据服务器提供的服务类型不同，分为文件服务器，数据库服务器，应用程序服务器，WEB服务器等。
2. web服务器环境：apache , ngnix , tomcat，nodejs
3. 服务器访问方式：localhost -> www.abc.com
    - 端口：服务器对外开放的门的门牌号
        * 80：http的默认端口
        * 443：https的默认端口
        * 3306：mysql的默认端口
        * 27017：mongoDB的默认端口
4. 数据库：mysql，sqlserver，mongoDB
    - 数据库：数字，字符
    - 磁盘（硬盘) 文件本身(图，视频,PDF)   文件服务器
4. 后台管理程序：
    - java，php，python，nodejs

# 二、开发方式
1. 大后端（前后端不分离）： 
    - 用户 -> 地址栏(http[s]请求) -> web服务器（收到) -> nodejs处理请求(返回静态、动态) -> 请求数据库服务(返回结果) -> nodejs(接收) -> node渲染页面 -> 浏览器（接收页面，完成最终渲染)
2. 大前端（前后端分离）： 
    - 前端 -> http[s]请求 -> web服务器（收到) -> nodejs处理请求(返回静态、动态) -> 请求数据库服务(返回结果) -> nodejs(接收) -> 返回给前端（ajax，form，fetch，axios）-> 前端处理数据(渲染) -> 浏览器（接收页面，完成最终渲染)

# 三、node基础
1.  认识Node.js 

> Node.js是一个javascript运行环境。它让javascript可以开发后端程序，实现几乎其他后端语言实现的所有功能，可以与PHP、Java、Python、.NET、Ruby等后端语言平起平坐。
>
> Nodejs是基于V8引擎，V8是Google发布的开源JavaScript引擎，本身就是用于Chrome浏览器的js解释部分，但是Node.js的作者Ryan Dahl，把V8引擎搬到了服务器上，用于做服务器的软件。
>

    - 作用：用来实现后台管理程序
    - 目的：数据服务，文件服务，web服务
    - 类似：php，.net，java(jsp) ....
2.  优势： 
    - Nodejs语法完全是js语法，只要你懂js基础就可以学会Nodejs后端开发
    - NodeJs超强的高并发能力，实现高性能服务器
    - 开发周期短、开发成本低、学习成本低
3. 使用 Node.js 需要了解多少 JavaScript

> [http://nodejs.cn/learn/how-much-javascript-do-you-need-to-know-to-use-nodejs](http://nodejs.cn/learn/how-much-javascript-do-you-need-to-know-to-use-nodejs)
>

4. 浏览器环境 和 Node.js环境

![1653931239327-e97fad89-5165-4c3c-b7a8-40bb56c59761.png](./img/PqdajREEXZBpnwRM/1653931239327-e97fad89-5165-4c3c-b7a8-40bb56c59761-549321.png)

Node.js 可以解析JS代码（没有浏览器安全级别的限制）提供很多系统级别的API，如：

    - 文件的读写 (File System)
    - 进程的管理 (Process)
    - 网络通信 (HTTP/HTTPS)

Node.js中只有JavaScript的ECMAScript部分，没有DOM和BOM！！！

5. 环境搭建：nodejs + web应用 + 数据库
    - 下载：[http://nodejs.cn/download/](http://nodejs.cn/download/)
        * windows7及以下：[https://nodejs.org/dist/v12.8.0/](https://nodejs.org/dist/v12.8.0/)
        * windows8及以上：[https://nodejs.org/dist/v18.12.0/node-v18.12.0-x64.msi](https://nodejs.org/dist/v18.12.0/node-v18.12.0-x64.msi)
        * Mac：[https://nodejs.org/dist/v18.12.0/node-v18.12.0.pkg](https://nodejs.org/dist/v18.12.0/node-v18.12.0.pkg)
    - 安装：next安装法
    - 测试安装成功：
        * windows：命令行工具
            + win+r -> 输入cmd，回车 -> 在命令行中输入：`node -v`，回车后查看node版本号
        * mac：终端
            + 输入：`node -v`，回车后查看node版本号
6. 版本介绍： 
    - Vx(主).x(子).x(修正)
    - 主：1/3的API发生巨变，使用方式变化了
    - 子：API没有删减，使用方式没变化，内部实现发生了变化
    - 修正版：什么都没变，处理一下bug 
        * V6.8.x 稳定
        * V6.9.x 非稳定版
        * Vx.x.x-beta 测试
        * vx.x.x-rc 测试稳定
        * vx.x.x-alpha 测试稳定
7. Node.Js的执行
    1. 命令行形式直接执行，执行的是具体的代码 
        1. 打开系统命令行工具（见下节命令行工具介绍）
        2. 在命令行工具中输入：`node`，回车，进入node编程状态
        3. 书写自己的代码，回车执行
        4. 退出node的编程状态： 
            + `ctrl+c`，两次
            + 输入`.exit`，回车
        * 执行node代码非常方便，但是不适合长时间留存代码，不适合代码量较多的功能
        * 仅仅适合用来对一些API进行测试
    2. 文件形式执行，执行的是文件 * 
        1. 编写node的文件：xxx.js 
            + 注意扩展名也是js
        2. 在 要执行的文件所在的文件夹 内，打开系统命令行工具（见下节命令行工具介绍）
            + 如：要执行的文件为`d:资料/node学习/code/day06/test.js`
            + 那么命令行的路径需要为：`d:资料/node学习/code/day06`
        5. 确保命令行工具没有在node编程状态
        6. 使用：`node 文件名`，回车执行
        * 适合长时间留存代码，适合代码量较多的功能
        * 适合投入实际项目使用
        * 注意：要使用node执行的文件的文件名，要遵守变量的命名规范
8. 系统命令行工具介绍
    - 打开方式 
        * windows：命令行窗口 
            1. 方法一： 
                - 按：win+r，打开运行窗口 -> 输入：cmd -> 回车
            2. 方法二： 
                - 开始菜单 -> 搜索：cmd -> 点击对应的命令行工具图标，打开
            3. 方法三：
                - 打开任意文件夹，在文件夹地址栏空白位置点击，输入cmd，可以在当前文件夹位置打开命令行工具
        * mac：终端 
            1. 在程序坞中，找到终端，点击打开
            2. 在任意文件夹身上右键，选择：服务->新建位于该文件夹位置的终端
                1. 可以直接在当前文件夹位置打开命令行工具
    - 路径切换 * 
        * windows： 
            + 打开上层文件夹：`cd ../`
            + 进入指定子文件夹：`cd 指定文件夹名` 
                - `cd 学习`
                - `cd d:资料/node学习/code/day06`
            + 切换盘符：`盘符:` 
                - `d:`或`c:`
            + 查看当前文件夹内的子文件：`dir` 
                - 帮助开发者查看接下来要去哪，或当前路径处于哪个位置
            + 清屏：`cls`
        * mac： 
            + 打开上层文件夹：`cd ../`
            + 进入指定子文件夹：`cd 指定文件夹名` 
                - `cd 学习`
                - `cd 资料/node学习/code/day06`
            + 查看当前文件夹内的子文件：`ls` 
                - 帮助开发者查看接下来要去哪，或当前路径处于哪个位置
            + 清屏：`clear`



# 四、Node.js的模块化
![1653932577656-7eeb17fa-5814-4f09-aea6-756cbf5fcfd8.png](./img/PqdajREEXZBpnwRM/1653932577656-7eeb17fa-5814-4f09-aea6-756cbf5fcfd8-704180.png)

+ Node.js的模块化标准采取的是CommonJS 规范。



1.  模块化的目的 
    - 主要为了JS在后端的表现制定
    - CommonJS 是个规范，Node.js / webpack 实现了这个规范
    - ECMAScript是个规范，JavaScript 实现了这个规范
2.  模块化方式 
    - 服务器端JS：相同的代码需要多次执行 | CPU和内存资源是瓶颈 | 加载时从磁盘中加载 
        * node模块： `http`,`fs`,`querystring`,`url`
        * 模块化方式：`require('模块名')`
    - 浏览器端js：代码需要从一个服务器端分发到多个客户端执行 | 带宽是瓶颈 | 通过网络加载 
        * 模块化方式：CMD`seajs.js`；AMD`require.js`；ES6: `export`/ `import`
3.  CommonJS模块化方式 
    -  require 引入模块、输入 
        * `require('模块名')` 
            + 不指定路径：先找系统模块 -> 再从项目环境找node_modules | bower_components (依赖模块)-> not found
            + 指定路径：指定路径 -> not found
        * require输入的是一个对象
    -  exports 导出，批量输出，都是属性 
        * `exports.自定义属性 = 值`
    -  module.exports 默认输出，只能输出一次，require输入的是任意接收 
        * `module.exports = { 自定义属性：值 }`
4. Node.js的模块分类
    1. 内置模块：官方提供，直接引入 
        * 引入：`const 变量 = require("模块名");`
        * 使用：`变量.xxx()`
        * 内置模块：`fs`，`url`，`querystring`，`path`
    2. 第三方模块：非官方，非当前开发者，需要先获取（下载）到第三方模块，才能引入 
        * 下载：使用包管理器 `npm`下载第三方模块
        * 引入：`const 变量 = require("模块名");`
        * 使用：`变量.xxx()`
    3. 自定义模块：自己写的模块，先定义模块，再引入模块 
        * 定义：`exports.xxx = 功能或对象`
        * 引入：`const 变量 = require("路径+模块名");`
        * 使用：`变量.zzz()`

# 五、包管理器 - npm
1. 介绍： 
    - 作用：安装模块（包），自动安装依赖，管理包（增，删，更新，项目所有包)
    - nodejs 环境自带 npm 包管理器
        * 官方文档：[https://www.npmjs.com/](https://www.npmjs.com/)；下载源地址：`https://registry.npmjs.org`
        * 淘宝镜像文档：[http://npmmirror.com](http://npmmirror.com)；下载源地址：`http://registry.npmmirror.com`
    - 切换淘宝镜像：`npm config set registry http://registry.npmmirror.com`
    - 类似的包管理器：yarn（[https://www.yarnpkg.cn/](https://www.yarnpkg.cn/)），cnpm，bower，pnpm等
2. 初始化npm环境（生成项目依赖描述文件：package.json）
    - `npm init`
    - `npm init -y`全部选项默认

```json
{
    "name": "myapp",				// 项目名称，不要和依赖的包重名
    "version": "0.0.1",			// 版本
    "description": "test and play",     // 项目描述
    "main": "index.js",     // 入口文件
    "dependencies": {       // 生产依赖，上线也要用
        "jquery": "^3.2.1",
        "express": "^4.17.1"
    },
    "devDependencies": {    // 开发依赖，上线就不用
        "node-sass": "^2.0.0"
    },
    "scripts": {            // 命令行
        "test": "命令行",
    },
    "repository": {         // 仓库信息
        "type": "git",
        "url": "https://gitee.com/liyangyf/hammer.git"
    },
    "keywords": [						// 项目关键词
        "test",'xx','oo'
    ],
    "author": "杨树林",			// 作者
    "license": "ISC",				// 认证
    "homepage": "https://gitee.com/liyangyf/hammer.git"		// 主页地址
}
```

3. 安装模块的环境位置 
    1.  全局环境：
        * 可以独立运行，或，在package.json的scripts内配置命令后，使用`npm run 命令名`执行
        * 如：命令行工具：nrm，脚手架工具：vue-cli，监听文件变化自动执行：nodemon
        * 安装或卸载命令
            + `npm install 包名 -g`
                - `install` 可简写为 `i`
            + `npm uninstall 包名 -g`
                - `uninstall` 可简写为 `uni`
    2.  项目环境
        * 如果是命令行工具，只能在package.json的scripts内配置命令后，使用`npm run 命令名`执行
        * 其他第三方模块，在模块内直接使用`require`函数引入即可
        * 依赖环境
            + 生产依赖dependencies：运行时的依赖，发布后，即生产环境下还需要用的模块。
                - 安装命令：`npm i 包名 --save` 简写为：`npm i 包名 -S`
                    * 如：jquery，express
            + 开发依赖devDependencies：开发时的依赖。只在开发过程中使用，发布后用不到。
                - 安装命令：`npm i 包名 --save-dev`简写为：`npm i 包名 -D`
                    * 如：node-sass，webpack
4. 常用npm命令
    - 列出所有已装包 
        * `npm list -g`（不加-g，列举当前目录下的安装包）
    - 检查安装的包是否已经过时
        * `npm outdated`
    - 查看当前包概要信息 
        * `npm info 包名`（详细信息）
        * `npm info 包名 version`（获取最新版本）
    - 安装package.json文件内已指定的所有包 
        * `npm install`
        * package.json文件内的版本约束：
            + `^x.x.x`      约束主版本，后续找最新
            + `~x.x.x`      保持前两位不变，后续找最新
            + `*`           最新
            + `x.x.x`       定死了一个版本
    - 安装指定版本的包：
        * `npm install 包名@版本号`
5. 自动刷新服务器插件：nodemon：
    - 下载：`npm i nodemon -g`
    - 使用：在命令行中：`nodemon 文件名`
    - 当使用nodemon执行的文件发生改变了，会自动重新执行该文件
6. 模块下载中，如果比较卡顿（超过5分钟），或者下载报错：
    - 再三确认自己的命令和包名没写错
    - `ctrl + c` -> `npm uni 包名`  -> `npm cache clean --force`(清除缓存) -> 换网 -> `npm i 包名`











配置第三方命令窗口的管理员权限

[https://blog.csdn.net/weixin_41636483/article/details/113376766?spm=1001.2014.3001.5501](https://blog.csdn.net/weixin_41636483/article/details/113376766?spm=1001.2014.3001.5501)



> 更新: 2023-09-18 10:00:51  
> 原文: <https://www.yuque.com/liyangyf/js/wp1fft>