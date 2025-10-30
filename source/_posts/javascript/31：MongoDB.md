---
title: MongoDB
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-16 09:00:00
---
# 31：MongoDB

# 一、介绍
+ MongoDB 是一个基于分布式文件存储的数据库管理系统。
+ 由 C++ 语言编写，是一个开源数据库系统。
+ 旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。
+ MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。
+ MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。
+ MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。
+ 在高负载的情况下，添加更多的节点，可以保证服务器性能。

#### 关系型数据库（mysql）与非关系型数据库（mongodb）的区别：
| **sql术语/概念** | **mongodb术语/概念** | **解释/说明** |
| --- | --- | --- |
| database | database | 数据库 |
| table | collection | 数据库表/集合 |
| row | document | 数据记录行/文档 |
| column | field | 数据字段/域 |
| index | index | 索引 |
| table joins | | 表连接，Mongodb不支持 |
| primary key | primary key | 主键,MongoDB自动将_id字段设置为主键 |


#### 了解两方的优缺点以及特性： 
|  | **关系型数据库** | **非关系型数据库** |
| --- | --- | --- |
| **特点** | 1. 关系型数据库是指采用了关系模型来组织数据的数据库；<br/>2. 关系型数据库的最大特点就是事务的一致性<br/>3. 简单来说，关系模型指的就是二维表格模型，关系型数据库就是由二维表及其之间的关联组成的数据组织 | 1. 使用键值对存储数据<br/>2. 分布式<br/>3. 不支持ACID特性<br/>4. 非关系型数据库严格上来说，不算是一种数据库，应该是一种数据结构化存储方法的集合 |
| **优点** | 1. 容易理解；<br/>2. 使用方便；<br/>3. 易于维护；<br/>4. 支持SQL，可用于复杂的查询 | 1. 无需经过sql层的解析，读写性能高；<br/>2. 基于键值对，数据没有耦合性，容易扩展；<br/>3. 存储数据的格式，nosql使用key:val的形式，文档的形式，图片形式等等，而关系型数据库则只支持基础类型 |
| **缺点** | 1. 为了维护一执行需要消耗大量的性能，影响读写<br/>2. 固定的表结构<br/>3. 高并发读写需求<br/>4. 海量数据的高效率读写 | 1. 不提供sql支持，学习成本高<br/>2. 无事务处理，附加功能和报表支持也不好 |


# 二、下载，安装和连接（启动）
1. [下载mongodb](https://www.mongodb.com/try/download/community)
    - 选择对应操作系统和版本的压缩包，下载完成后，解压到自定义的安装目录（记住这个目录路径）

![1654852420105-6dc4d05d-f1eb-4439-a26b-4365eb6e9ec7.png](./img/VY7PkuobyoaPvvc9/1654852420105-6dc4d05d-f1eb-4439-a26b-4365eb6e9ec7-974631.png)![1654852533348-e5c313a3-7e41-40d9-b58b-b5b081df15a2.png](./img/VY7PkuobyoaPvvc9/1654852533348-e5c313a3-7e41-40d9-b58b-b5b081df15a2-935185.png)

    - 注意：**win7系统，需要修改成**`**4.0.28**`**版本**
2. 手动连接mongoDB数据库（注：【免安装版】需要手动连接，【安装版】自动连接，不需要手动连接）
    - 2.1. 在任意盘符下创建新文件夹（data），在新文件夹内，再创建新文件夹（db）
    - 2.2. 在mongo的安装目录中进入bin文件夹，打开命令提示符（命令提示符中路径必须为当前bin目录，最好以管理员方式打开命令提示符）
    - 2.3. 在命令提示符中输入：`./mongod --dbpath d:\data\db` 
        * 在出现的众多提示中，如果出现：waiting for connections on port 27017。表示连接成功
    - 2.4. 切记：当前命令提示符不要关闭，重新在bin目录下打开一个命令提示符
    - 2.5. 在新打开的命令提示符中输入：`./mongo`。进入mongo操作状态
    - 2.6. 开始mongodb数据库的命令操作
    - ...
3. 注意：以下mongo命令都是在进入mongo操作状态后使用

# 三、mongoDB的常用指令（了解）
### 1. 查看数据库的帮助文档
+ 在操作数据库之前，可以先查看mongodb数据库提供的帮助文档

| 命令 | 说明 |
| ---: | :--- |
| help | 帮助文档 |
| db.help() | 数据库帮助文档 |
| db.test.help() | 数据库集合帮助文档 |
| db.test.find().help() | 数据库集合查询帮助文档 |


### 2. 数据库操作
1. 创建并选择：（有则选，无则建）
    - `use dbName`
2. 查询
    - `db`
        * 查看当前数据库
    - `show dbs`
        * 查看所有数据库
    - 注：在查看所有数据库时，不显示空数据库
3. 显示数据库信息
    - `db.stats()`
        * 查看数据库的状态信息
    - `db.getMongo()`
        * 查看数据库的链接地址
    - `db.version()`
        * 查看数据库的版本信息
    - `db.getName()`
        * 查看当前数据库名称
4. 删除数据库 -- **慎用**
    - `db.dropDatabase()`

### 3. 集合的操作<font style="color:rgb(51, 51, 51);">（下文中所有的colName，为具体要操作的集合名）</font>
1. 创建集合 
    - `db.createCollection(name)`
        * name：集合名，字符
2. 删除集合：
    - `db.colName.drop()`
3. 查看所有集合：
    - `show collections`
4. 查看当前数据库中所有的集合名称
    - `db.getCollectionNames()`

### 4. 数据的操作<font style="color:rgb(51, 51, 51);">（下文中所有的colName，为具体要操作的集合名）</font>
1. 用户信息数据设计：
    - 用户名：username
    - 密码：password
    - 年龄：age
    - 籍贯：city

> 如：罗辑，123456，30，北京  
{username:"罗辑",password:"123456",age:30,city:"北京"}
>

2. 增
    - 插入单条：`db.colName.insert({})`
    - 插入多条：`db.colName.insert([{},{},...])`
    - 插入单条：`db.colName.insertOne({})`
    - 插入多条：`db.colName.insertMany([{},{},…])` ***
3. 删
    - 根据指定的键值对条件：
    - 删除单条数据：`db.colName.deleteOne({key:val})`
    - 删除多条数据：`db.colName.deleteMany({key:val})`
    - 删除所有数据：`db.colName.deleteMany({})`
4. 改
    - 根据指定的键值对条件：
    - 修改单条数据：`db.colName.updateOne({key:val},{$set:{key1:newVal,key2:newVal}})`
    - 修改多条数据：`db.colName.updateMany({key:val},{$set:{key1:newVal,key2:newVal}})`
        * Num为正自增，为负自减
    - 自增/自减单条数据：`db.colName.updateOne({key:val},{$inc:{key1:num}})`
    - 自增/自减多条数据：`db.colName.updateMany({key:val},{$inc:{key1:num}})`



### 5. 数据的操作----查
1. 基本查询所有数据：
    - `db.colName.find()`
2. 格式化查询所有数据：
    - `db.colName.find().pretty()`
3. 指定键值对条件查询：
    - `db.colName.find({key:val})`
4. 指定条件查询（可以为{}表示所有数据），并限制字段显示：
    - `db.colName.find({key:val},{userName:1, pass:1})`
        * inclusion模式，指定返回的键，不返回其他键
    - `db.colName.find({key:val},{userName:0, pass:0})`
        * exclusion模式，指定不返回的键，返回其他键
    - 注意：_**id默认返回，如果不需要，需主动设置 _id:0**
5. 分页查询：
    - `db.colName.find({key:val}).limit(num).skip(start)`
        * num：表示个数
        * start：表示起始索引，默认为0
    - 从第index*num也开始查询，查询num条，index为当前页码的索引
6. 排序查询：
    - `db.colName.find({key:val}).sort({key:1})`
        * 1升序，-1降序
7. 区间查询： - 价格区间
    - `db.colName.find({ key: {$lt:val1, $gt:val2} })`
        * 小于val1，大于val2
    - `db.colName.find({ key: {$lte:val1, $gte:val2} })`
        * 小于等于val1，大于等于val2
8. 模糊查询： - 搜索
    - `db.colName.find({ key: /val/})`
        * 查询key中包含val的数据
    - `db.colName.find({ key: /^val/})`
        * 查询key中包含val且以val开头的数据
9. 或查询： - 用户名或手机号登录
    - `db.colName.find({ $or: [{key1:val1},{key2:val2}] })`
        * 查询key1为val1或key2为val2的数据
10. 且查询：
    - `db.colName.find({ key1:val1, key2:val2 })`
        * 查询key1为val1且key2为val2的数据
11. 获取指定字段的数据： - 分类
    - `db.colName.distinct("key")`
        * 获取指定字段的所有数据，去重并以数组的形式返回



# 四、node操作数据库（重点）
### 1. 安装Node对mongodb的支持
+ NodeJS中MongoDB驱动：mongoose
+ 下载并安装mongoose 
    - `npm install mongoose -S`



### 2. 连接并选择数据库（db.js）
1.  引入mongoose模块 
    - `const mongoose = require("mongoose");`
2.  连接mongodb并选择指定数据库：dbName 
    - `mongoose.connect("mongodb://localhost:27017/dbName");`
3.  连接成功 
    - `mongoose.connection.on("connected",()=>{ })`
4.  连接断开 
    - `mongoose.connection.on("disconnected",()=>{ })`
5.  连接错误 
    - `mongoose.connection.on("error",()=>{ })`
6.  连接成功之后，将该模块暴露出来： 
    - `module.exports = mongoose`



### 3. 创建集合（users.js）
1.  引入数据库的连接 
    - `const mongoose = require("./db.js")`
2.  创建集合需要使用的通用对象 
    - `const Schema = mongoose.Schema;`
3.  说明集合需要使用的字段和类型 

```javascript
const userSchema = new Schema({
    userId:{type:String},
    userName:{type:String},
    pass:{type:String},
    age:{type:Number},
    city:{type:String},
    tel:{type:String}
})
```

4. 创建集合后，将该模块暴露出来 
    - `module.exports = mongoose.model("users", userSchema);`



### 4. 插入数据（insert.js）
1.  引入创建集合（在创建集合中，引入了连接数据库） 
    - `const User = require("./users.js");`
2.  插入语法： 
    -  插入单条数据：`User.insertMany({}).then( res=>{} )` 
    -  插入多条数据：`User.insertMany([{},{},…]).then( res=>{} )` 
    -  注意：数据操作，属于异步操作，需要在回调函数中查看插入结果 



### 5. 修改数据（update.js）
1.  引入创建集合（在创建集合中，引入了连接数据库） 
    - `const User = require("./users.js");`
2.  修改语法： 
    - 修改一条：`User.updateOne({key: val}, {kay: val}).then( res=>{} )`
    - 修改一条（自增/自减）：`User.updateOne({key: val}, {$inc: {kay: val}}).then( res=>{} )`
    - 修改多条：`User.updateMany({key: val}, {kay: val}).then( res=>{} )`
    - 修改多条（自增/自减）：`User.updateMany({key: val}, {$inc: {kay: val}}).then( res=>{} )`



### 6. 删除数据（delete.js）
1.  引入创建集合（在创建集合中，引入了连接数据库） 
    - `const User = require("./users.js");`
2.  删除语法： 
    - 删除一条：`User.deleteOne({key: val}).then( res=>{} )`
    - 删除多条：`User.deleteMany({key: val}).then( res=>{} )`
    - 删除全部：`User.deleteMany({}).then( res=>{} )`



### 7. 查询数据（find.js）
1.  引入创建集合（在创建集合中，引入了连接数据库） 
    - `const user = require("./user.js");`
2.  查询语法： 
    - 查找全部： 
        * `user.find( {}, {} ).then( res=>{} )` 
            + 参数1：对象，为查询条件
            + 参数2：对象，要返回的字段
            + 参数3：函数，查询结束后执行 
                - err：报错信息
                - data：查询到的数据
    - 排序查：
        * `user.find( ).sort( {age:1} ).then( res=>{} )`
            + 1为升序，-1为降序
    - 分页查：
        * `user.find( ).limit( num ).skip( start ).then( res=>{} )`
            + num：为单页条数
            + start：为起始索引，分页查询时一般为：页码 * 个数
    - 区间查找： 
        * `user.find( {key: {$lte:maxVal, $gte:minVal}} ).then( res=>{} )`
    - 模糊查找： 
        * `user.find( {key: /^张/} ).then( res=>{} )`
    -  查找数据总数量： 
        * `user.countDocuments( ).then( res=>{} )`
    -  查找指定字段的数据： - 分类 
        * `user.distinct( "字段名" ).then( res=>{} )`