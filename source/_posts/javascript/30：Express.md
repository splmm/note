---
title: Express
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-15 20:00:00
---
# 30：Express

# 一、Express的安装和基本使用
1. 介绍： 
    - [Express](https://www.expressjs.com.cn/) 是基于Node.Js平台，快速，开放，极简的Web开发框架。
    - 是Node.Js的第三方模块，需要安装之后才能使用 
    - express的特点就是对原生NodeJs做了二次封装，非侵入式。 
2. 准备 
    - 首先准备好项目目录：
        * 新建文件夹，在当前目录下打开命令提示符，使用`npm init`创建项目配置文件package.json
    - [安装Express](https://www.expressjs.com.cn/starter/installing.html)：
        * `npm i express -S`
3. [安装之后，需要在文件引入才能使用](https://www.expressjs.com.cn/starter/hello-world.html)： 
    - `const express = require("express");`
        * express自身是一个函数
    - `const app = express();`
        * 执行之后，得到express对象（服务对象）
    - `app.listen(端口, ()=>{})`
        * 开启指定端口的监听，开启成功后执行回调函数
4. express基础使用：
    - `app.use(路由, (req, res, next)=>{});`
        * 当接收到指定请求路由之后，并执行对应的回调函数，一般用于注册中间件
    - `app.all(路由, (req, res, next)=>{});`
        * 只能接收指定请求路由的所有动作请求，并执行对应的回调函数
    - `app.get(路由, (req, res, next)=>{});`
        * 只能接收指定请求路由的get请求，并执行对应的回调函数
    - `app.post(路由, (req, res, next)=>{});`
        * 只能接收指定请求路由的post请求，并执行对应的回调函数
5. express对相应请求的方法内的req和res对象做了增强：
    - req表示前端到后台的信息
    - res表示后台到前端的信息
    - 保留了原有属性和方法：如req.url/req.method/res.write/res.end
    - 增加了：
        * `res.send()`
            + 返回数据并结束请求
        * `req.query`
            + 接收get数据，对象类型
        * `req.body`
            + 接收post数据，对象类型（需要使用中间件body-parser解析）

# 二、什么是中间件
1. Express 是一个自身功能极简，完全是由路由和中间件构成一个的 web 开发框架：从本质上来说，一个 Express 应用就是在调用各种中间件。 
2. 中间件（Middleware） 是一个函数或对象，它封装了express的功能，可以访问请求对象（request object (req)）、响应对象（response object (res)）、和 web 应用中处于请求 - 响应循环流程中的中间件，一般被命名为 next 的变量。中间件相对于express来说，是整个流水线上的一环。
3. 中间件的功能包括：
    - 执行任何代码。
    - 修改请求和响应对象。
    - 终结请求-响应循环。
    - 调用堆栈中的下一个中间件。
4. 如果当前中间件没有终结请求-响应循环，则必须调用 next() 方法将控制权交给下一个中间件，否则请求就会挂起。
5. express中间件的使用：
    - 内置中间件：`express.static`，是 Express 内置的中间件。负责提供静态资源托管，每个应用可有多个静态目录

```javascript
app.use(express.static('./public'))
app.use(express.static('./uploads'))
app.use(express.static('./files'))
```

    - 内置中间件：`express.urlencoded`，是 Express 内置的中间件。负责提供post方式下的body数据解析

```javascript
app.use(express.urlencoded({extended:false}))
```

    - 第三方中间件 - 需要安装后才能使用：
        * cookie-parser中间件：
            + 下载中间件：（解析cookie数据）
                - `npm install cookie-parser`
            + 引入中间件
                - `const cookieParser = require("cookie-parser");`
            + 使用并将中间件绑定到express的app对象
                - `app.use( cookieParser() )`

# 三、express的路由介绍
1. 什么是路由：就是根据不同的路由地址，响应不同的内容。一般情况下，在项目开始之前，都会对路由地址进行规划、设定，然后在项目开发过程中，针对预先规划的路由地址进行处理和响应。
2. 路由是指，确定应用程序，如何响应对特定端点的客户端请求，该特定端点是URI（或路径）和特定的HTTP请求方法（GET，POST等）。 
3. 每个路由可以具有一个或多个处理函数，这些函数在匹配该路由时执行。 
4. express的路由设置语法：
    - `app.METHOD(PATH, HANDLER)`
        * app是express的模块对象
        * METHOD是小写的请求方式，如：get，post
        * PATH是服务器上的路由地址
        * HANDLER是匹配路由时执行的功能函数
4. 如：

```javascript
app.get('/', function (req, res, next) {
    console.log(1);
    next()
})
app.get('/', function (req, res, next) {
  	console.log(2);
    res.send('this is GET request')
})

app.post('/', function (req, res, next) {
    res.send('this is POST request')
})
```

# 四、Express中request和response的属性封装
#### request
+ req.app：当callback为外部文件时，用req.app访问express的实例
+ req.baseUrl：获取路由当前安装的URL路径
+ req.cookies：获取cookie数据，需要配合cookie-parser中间件，才能解析cookie数据
+ req.query：获取get数据
+ req.body：获取post数据，需要提前注册中间件
    - `app.use(express.urlencoded({extended:false}))`
    - `app.use(express.json())`
+ req.fresh / req.stale：判断请求是否还「新鲜」
+ req.hostname / req.ip：获取主机名和IP地址
+ req.originalUrl：获取原始请求URL
+ req.path：获取请求路径
+ req.protocol：获取协议类型
+ req.route：获取当前匹配的路由
+ req.subdomains：获取子域名
+ req.accepts()：检查可接受的请求的文档类型
+ req.acceptsCharsets / req.acceptsEncodings / req.acceptsLanguages：返回指定字符集的第一个可接受字符编码
+ req.get()：获取指定的HTTP请求头
+ req.is()：判断请求头Content-Type的MIME类型



#### response
+ res.app：同req.app一样
+ res.append()：追加指定HTTP头
+ res.set()在res.append()后将重置之前设置的头
+ res.cookie(name，value [，option])：设置Cookie
    - opition: domain / expires / httpOnly / maxAge / path / secure / signed
+ res.clearCookie()：清除Cookie
+ res.download()：传送指定路径的文件
+ res.get()：返回指定的HTTP头
+ res.send()：向前端响应数据（字符，对象），并结束本次响应
+ res.location()：只设置响应的Location HTTP头，不设置状态码或者close response
+ res.render(view,[locals],callback)：渲染一个view，同时向callback传递渲染后的字符串，如果在渲染过程中有错误发生next(err)将会被自动调用。callback将会被传入一个可能发生的错误以及渲染后的页面，这样就不会自动输出了。
+ res.sendFile(path [，options] [，fn])：传送指定路径的文件 -会自动根据文件extension设定Content-Type
+ res.set()：设置HTTP头，传入object可以一次设置多个头
+ res.status()：设置HTTP状态码
+ res.type()：设置Content-Type的MIME类型
+ res.redirect()：路由重定向。后端主动修改路由地址，等同于前端的location.assign()



# 五、项目的开发方式
1. 前后端分离的项目（BSR）
    - 前端：页面渲染
    - 数据请求工具：form，ajax，......
    - 后端：数据处理
    - 后端管理数据库，处理数据，将数据处理成json后，通过媒介发给前端，前端接收到后端的数据，处理之后，渲染到前端页面
    - 前后端可以同时开发
2. 前后端不分离的项目（SSR）
    - 前端：静态页
    - 后端：数据处理，将前端的静态页改成页面模版后，渲染
    - 前端先将静态页准备好，发（U盘，QQ，仓库共享）给后端，后端将前端页面改成页面模版，后端再去请求数据，处理数据，将数据渲染到模版，将模版渲染到浏览器
3. 页面模版是什么：在静态页中插入后端的数据变量，逻辑语句，通过后端程序执行，得到真实的页面，常见的页面模板：jade，ejs