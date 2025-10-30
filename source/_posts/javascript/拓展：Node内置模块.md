---
title: Node内置模块
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-17 20:00:00
---
# 拓展：Node内置模块

## 1. http模块
+ 可以实现搭建服务器，请求数据等功能
1. 引入模块：`const http = require('http');`
2. 创建服务： 
    - `const server = http.createServer((request, response)=>{});`  // 返回http对象 
        * require：请求：浏览器 -> 服务器 
            + `req.url`地址，提取地址栏数据
            + `req.on('data|end');`提取非地址栏数据，所有的http[s]都会触发data事件和end事件
        * response：响应：服务器 -> 浏览器 
            + 响应头设置：`res.writeHead(200,{'Content-Type':'text/html;charset=utf-8'});`
            + `res.write(字符/数据<string><buffer>);`
            + `res.end();` 结束响应
3. 监听： 
    - `server.listen(端口，[地址]，[回调函数]);`
        * 端口：1-65535，1024以下系统占用，80
        * 地址（可选）：
            + 虚拟域名：localhost或127.0.0.1
            + 真实域名：xx.duapp.com
        * 回调（可选）：监听成功，执行回调函数
4. 小提示： 
    - 服务器代码更新后，需要重启服务器 
        * 自动重启工具：`nodemon`
        * 可在命令行中通过npm全局安装该模块：`npm install nodemon -g`
        * 使用：`nodemon 文件`
    - 接口测试工具 
        * postman
5. 服务端发起get请求：`http|https.get("url", 回调);`

```javascript
const https = require("https");

const url = "https://movie.douban.com/j/search_subjects?type=movie&tag=热门&page_limit=50&page_start=0";

https.get(url, res=>{
  let data = "";
  res.on('data', chunk=>{
    data += chunk;
  })
  res.on('end', ()=>{
    console.log(data);
  })
})
```

6. 服务端发起post请求：`http|https.request(ops, 回调);`

```javascript
const https = require("https");
const url = require("url");

const urlObj = url.parse("https://music.163.com/weapi/song/enhance/player/url/v1");

const options = {
  hostname:urlObj.hostname,
  port:443,
  method:"POST",
  path:urlObj.pathname,
  headers:{
    "Content-Type": "application/x-www-form-urlencoded"
  }
};

const req = https.request(options, res=>{
  let data = "";
  res.on('data', chunk=>{
    data += chunk;
  })
  res.on('end', ()=>{
    console.log(data);
  })
})

req.write("params=HBeWY9slvubV%2FsCFfS%2BUhfCTNEELGpEkyepAFVjDOXhiyngTesa1d9DqULe%2BG9EVcqZvNbDczEMZCCwv5G%2BCDFQUMrXNR%2BGwYg93iAybBw76XBRXiNmwSo088BTbOiwyD%2Fz5myLxHlCOrcrjzGZOFQ%3D%3D&encSecKey=82ea0a9c0217e745c8009280044eb9badf7cbb8d79c610015818721d0db9102c9a1d5c4fd062d331cd7aed0973ad3ddc80d29381de962d56e1df3584b6e65e6497aa8f3c133b2890cef54f3a29883219de94b82ffd30291c85e3d2353e4cea8bce3a8f643c9399ac4161dc72d7e9e83871ced1610fa5a1c2e6a25e00c0e9cc6c");
req.end();
```



## 2. fs模块 -  file system文件服务
1. 引入模块：`const fs = require('fs');` 
2. 异步读文件：`fs.readFile('路径',[配置],回调(err,data))` 
    - err：报错信息，null或者undefined，表示没有错误 
    - data：二进制buffer流 ，可以通过`data.toString("utf-8")`转换格式
3. 同步读文件：`let data = fs.readFileSync('路径',[配置])` 
    - 免去回调地狱，但会阻塞其他程序执行，比较占用时间
    - 建议使用`try-catch`语句捕获同步语句的错误
4. 异步写文件：`fs.writeFile('路径','内容',回调(err))` 
    - err：保存信息，null或者undefined，表示没有错误
5. 同步写文件：`let data = fs.writeFileSync('路径','内容')` 
    - 免去回调地狱，但会阻塞其他程序执行，比较占用时间
6. 删除文件：`fs.unlink('路径',回调)` 
7. 文件重命名：`fs.rename('老文件名','新文件名',(err)=>{})` 
8. 实现静态资源托管: 
    - 什么是静态资源： css/html/js/图片/json/字体...
    - 前端资源请求: 
        * `href``src``locaction.href`
    - 后端资源读取：  
`fs.readFile(文件名,[编码],回调(err,data));`

## 3. url模块 - 地址
1. 作用：解析或转换url数据
2. 引入：`const url = require("url");`
3. 使用： 
    - 字符转对象 
        * `const str = "http://localhost:8002/aaa?username=sdfsdf&content=234234#title4";`
        * `url.parse(str, true)` 
            + true 处理query数据，直接转成对象
        * obj参数 
            + protocol: 'http:'，  协议
            + slashes: true，  双斜杠
            + auth: null，  作者
            + host: 'localhost:8002'，  主机 www.baidu.com
            + port: '8002'，  端口
            + hostname: 'localhost'，  baidu
            + hash: '#title'，  哈希（锚)
            + search: '?username=sdfsdf&content=234234'，  数据
            + query: 'username=sdfsdf&content=234234'，  数据
            + pathname: '/aaa'，  文件路径
            + path: '/aaa?username=sdfsdf&content=234234'，  文件路径
            + href: 'http://localhost:8002/aaa?username=sdfsdf&content=234234#title'
    - 对象转字符 
        * `url.format(obj)`

## 4. querystring 模块 - 查询字段
1. 作用：处理query数据 
2. 引入：`const querystring = require("querystring");`
3. 字符转对象： 
    - `const str = "key=value&key2=value2";`
    - `querystring.parse(str)`
4. 对象转字符 
    - `const obj = {a:10, b:20}`
    - `querystring.stringify(obj)`
5. <font style="color:rgb(51, 51, 51);">escape/unescape</font>
    - 对数据中的中文或特殊符号进行转义，防止脚本注入攻击
    - `const str = 'id=3&city=北京&url=https://www.baidu.com';`
    - `querystring.escape(str);`
    - `const str = 'id%3D3%26city%3D%E5%8C%97%E4%BA%AC%26url%3Dhttps%3A%2F%2Fwww.baidu.com';`
    - `querystring.unescape(str);`

## 5. path模块 - 路径
1. 作用：处理路径数据
2. 引入：`const path = require("path");`
3. 合并路径 - 使用特定于平台的分隔符作为分隔符将所有路径部分连接，生成规范路径
    - `const str = path.join("aa", "qwe", "hello", "index.html");`
    - MAC：`aa/qwe/hello/index.html`
    - Windows：`aa\\qwe\\hello\\index.txt`

## 6. 关于路径的全局变量
1. 全局变量：`__dirname`
    - 用户获取当前文件所在文件夹的绝对路径
2. 全局变量：`__filename`
    - 用户获取当前文件的绝对路径