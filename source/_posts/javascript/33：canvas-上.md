---
title: canvas - 上
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-16 13:00:00
---
# 33：canvas - 上

# 一、介绍
1. canvas 是 HTML5 新增的一个标签，表示画布
    - `<canvas></canvas>`
2. canvas 也是 HTML5 的画布技术，可以通过编码的方式在画布上描绘图像

```html
<html>
  <head>
    ...
  </head>
  <body>
    <canvas></canvas>
  </body>
</html>
```

    - canvas 默认是一个行内块元素
    - canvas 默认画布大小是 300 * 150 
    - canvas 默认没有边框, 背景默认为无色透明

## 1.1 canvas 画布大小
1. 在绘图之前, 先要确定一个画布的大小
    - 因为画布默认是按照比例调整
    - 所以我们调整宽度或者高度的时候, 调整一个, 另一个自然会按照比例自己调整
    - 我们也可以宽高一起调整
2. 调整画布大小有两种方案
    - 第一种 : 通过 css 样式 ( 不推荐 )
    - 第二种 : 通过标签属性 ( 推荐 )
        * `<canvas width="1000" height="500"></canvas>`
3. 两种方案的区别
    - 通过 css 样式的调整方案（不推荐）
        * 因为 css 并没有设置了画布的大小，而是把原来 300 * 150 的画布的可视窗口变成了 1000 * 500。所以真实画布并没有放大, 只是可视程度变大了
        * 如：把一个 300 * 150 的图片，放大到 1000 * 500 的大小来看

![1688314621039-36926b7b-2708-45b9-bcde-29dbd88a25d4.png](./img/rNy4AFfuoNA4jJ2N/1688314621039-36926b7b-2708-45b9-bcde-29dbd88a25d4-808131.png)

    - 通过属性的调整方案（推荐）
        * 这才是真正的将画布大小调整到 1000 * 500

![1688314620961-c9b0aac9-af5f-4876-8d9d-ccbb24ae8041.png](./img/rNy4AFfuoNA4jJ2N/1688314620961-c9b0aac9-af5f-4876-8d9d-ccbb24ae8041-129071.png)

## 1.2 画布的坐标
    - canvas 画布和 css 的坐标系一样。左上角为 0 0 ，向右向下延伸为正方向

![1688314620948-acb00b43-4414-415f-81bd-30dce1a69c89.png](./img/rNy4AFfuoNA4jJ2N/1688314620948-acb00b43-4414-415f-81bd-30dce1a69c89-203694.png)

# 二、canvas 初体验
1. canvas 画布很简单，类似于 windows 电脑上的画板工具

![1688314620953-2018d011-d782-4e2b-9425-6d901b3fb835.png](./img/rNy4AFfuoNA4jJ2N/1688314620953-2018d011-d782-4e2b-9425-6d901b3fb835-760910.png)

    - 在绘制之前，先选定一个形状工具（直线，矩形，圆形，...）
    - 确定路径起点，落笔
    - 移动到路径终点，抬笔
    - 设定样式（粗细，颜色）
2. 在 canvas 绘制也是一样的逻辑
    - 创建一个画布工具箱
        * 语法：`canvas元素.getContext('2d')`
        * 如：`const ctx = canvasEle.getContext('2d')`
    - 确定路径起点，落笔
        * 将画笔移动到一个指定位置下笔
        * 语法：`工具箱.moveTo(x轴坐标, y轴坐标)`
        * `ctx.moveTo(100, 100)`
    - 移动到路径终点，抬笔
        * 将画笔移动到一个指定位置，画下一条轨迹（路径）
        * 注意：这里暂时没有显示，因为只是画了一个轨迹（路径）
        * 语法：`工具箱.lineTo(x轴坐标, y轴坐标)`
        * `ctx.lineTo(300, 100)`
        * 如果多个lineTo方法连续执行，表示连续绘制，即在本次抬笔的位置直接落笔。
    - 设定路径样式（可在落笔之前设置）
        * 语法：`工具箱.样式属性 = 样式值`
        * 线的宽度：`ctx.lineWidth = 10`
        * 线的颜色：`ctx.strokeStyle = '#000'`
    - 确定本次绘制的路径信息（生效，显现绘制效果）
        * `ctx.stroke()`
        * 必须绘制完成后执行
    - 如：绘制一条线段：

```javascript
// 0. 获取 canvas 标签元素
const canvasEle = document.querySelector('#canvas')

// 1. 创建画布工具箱
// 语法: canvas元素.getContext('2d')
const ctx = canvasEle.getContext('2d')

// 2. 开始绘制（落笔）
// 语法: 工具箱.moveTo(x轴坐标, y轴坐标)
ctx.moveTo(100, 100)

// 3. 将笔移动到一个指定位置, 画下一条轨迹（抬笔）
// 注意: 这里是没有显示的, 因为只是画了一个轨迹
// 语法: 工具箱.lineTo(x轴坐标, y轴坐标)
ctx.lineTo(300, 100)

// 4. 设定本条线的样式
// 设定线的宽度
// 语法: 工具箱.lineWidth = 数字
ctx.lineWidth = 10
// 设定线的颜色
// 语法: 工具箱.strokeStyle = '颜色'
ctx.strokeStyle = '#000'

// 5. 确定本次绘制信息
// 把上边画下的痕迹按照设定好的样式描绘下来
// 语法: 工具箱.stroke()
ctx.stroke()
```

    - 从坐标 ( 100, 100 ) 绘制到坐标 ( 300, 100 )
    - 线段长度为 200px
    - 线段宽度为 10px
    - 线段颜色为 '#000' ( 黑色 )

![1688314621027-51c42b15-7374-4090-a0c7-73ac77d1397c.png](./img/rNy4AFfuoNA4jJ2N/1688314621027-51c42b15-7374-4090-a0c7-73ac77d1397c-133541.png)

# 三、canvas 线宽颜色问题
+ 需要注意，在绘制任何图形时，尽量不要出现奇数宽度，因为canvas在划分了坐标点后，每次绘制都是绘制在点坐标上。
+ 一个宽度为 1px 的线段就会以如下这种方式被画出来：

![1688524066781-08822e59-2798-498c-8733-594438ffe6eb.png](./img/rNy4AFfuoNA4jJ2N/1688524066781-08822e59-2798-498c-8733-594438ffe6eb-276744.png)

+ canvas在描绘这个线段的时候，会把线段的最中心点放在这个像素点位上
+ 也就是说，在描述线宽的时候，实际上会从 0.5px 的位置绘制到 1.5px 的位置，合计描述宽度为 1px
+ 但是浏览器不能识别小数像素
+ 也就是说浏览器没办法从 0.5 开始绘制，也没有办法绘制到 1.5 停止
+ 那么就只能是从 0 开始绘制到 2。所以线宽就会变成 2px 了
+ 又因为本身一个像素的黑色被强制拉伸到两个像素宽度，所以颜色就会变浅
+ 就像我们一杯墨水, 倒在一个杯子里面就是黑色
+ 但是到在一个杯子里面的时候, 又倒进去一杯水, 颜色就会变浅
+ 所以最终呈现出来的样式，如下图：

![1688524261485-51957ba7-d598-46ec-9dd5-911d75fdd7c8.png](./img/rNy4AFfuoNA4jJ2N/1688524261485-51957ba7-d598-46ec-9dd5-911d75fdd7c8-395750.png)

+ 所以，我们在进行 canvas 绘制时，涉及到线段的宽度时，**一般不会把线段宽度设置成奇数，尽量设置为偶数**

# 四、线段的开始与闭合
1. 在绘制两条样式不同的独立线段时，如果直接进行绘制，后绘制的线段样式可能会影响先绘制的线段样式，如：

```javascript
const canvas = document.querySelector(".mycanvas");

const ctx = canvas.getContext("2d");

ctx.moveTo(100, 100);
ctx.lineTo(200, 100);
ctx.lineWidth = 10;
ctx.strokeStyle = "red";
ctx.stroke();

ctx.moveTo(100, 200);
ctx.lineTo(200, 200);
ctx.lineWidth = 4;
ctx.strokeStyle = "#000";
ctx.stroke();
```

+ 最终呈现出下图样式

![1688526291274-853f49f1-17c3-4362-a20e-22b85508fb76.png](./img/rNy4AFfuoNA4jJ2N/1688526291274-853f49f1-17c3-4362-a20e-22b85508fb76-486521.png)

+ 可以看到第二条线段的样式覆盖在了第一条线段上方
+ 这是因为在绘制一条新的线段之前，需要先初始化工具箱中的样式设置。
+ 也就是所谓的，准备绘制一条新的线段：`ctx.beginPath();`

```javascript
const canvas = document.querySelector(".mycanvas");

const ctx = canvas.getContext("2d");

ctx.beginPath();
ctx.moveTo(100, 100);
ctx.lineTo(200, 100);
ctx.lineWidth = 10;
ctx.strokeStyle = "red";
ctx.stroke();

// 设置线段的开始（初始化工具箱）
ctx.beginPath();
ctx.moveTo(100, 200);
ctx.lineTo(200, 200);
ctx.lineWidth = 4;
ctx.strokeStyle = "#000";
ctx.stroke();
```

+ 绘制效果：

![1688526596419-093db9af-5abb-458f-82a2-2b6fa9bea0f1.png](./img/rNy4AFfuoNA4jJ2N/1688526596419-093db9af-5abb-458f-82a2-2b6fa9bea0f1-837236.png)

2. 还可以通过绘制连续线段，组合成几何图形，如三角形：

```javascript
const canvas = document.querySelector(".mycanvas");

const ctx = canvas.getContext("2d");

ctx.beginPath();

ctx.moveTo(100, 100);
ctx.lineTo(200, 100);
ctx.lineTo(100, 200);
ctx.lineTo(100, 100);

ctx.lineWidth = 8;
ctx.strokeStyle = "red";

ctx.stroke();
```

绘制效果：

![1688527237254-e3dd96d2-35b4-4944-8685-7de5b73783c7.png](./img/rNy4AFfuoNA4jJ2N/1688527237254-e3dd96d2-35b4-4944-8685-7de5b73783c7-731883.png)

+ 注意细节：

![1688527438866-ca601ddd-ea91-478c-aedb-a20814e254e8.png](./img/rNy4AFfuoNA4jJ2N/1688527438866-ca601ddd-ea91-478c-aedb-a20814e254e8-041987.png)

+ 当最后一条线段的终点和第一条线段的起点重复时，并没有看到一种“闭合”的效果。
+ 这就需要我们主动设置连续线段的闭合：`ctx.closePath()`
+ 只要已经绘制了至少两条连续线段，就可以使用closePath进行闭合，它会自动将最后一条线段的终点和第一条线段的起点进行连接

```javascript
const canvas = document.querySelector(".mycanvas");

const ctx = canvas.getContext("2d");

ctx.beginPath();

ctx.moveTo(100, 100);
ctx.lineTo(200, 100);
ctx.lineTo(100, 200);
ctx.lineTo(100, 100);		// 本次绘制可以省略，亦可出现三角形效果

ctx.lineWidth = 8;
ctx.strokeStyle = "red";

// 闭合连续线段
ctx.closePath();

ctx.stroke();
```

+ 绘制效果：

![1688527835440-3296f61b-3dbc-4be7-bce6-fcda7f3a2ab4.png](./img/rNy4AFfuoNA4jJ2N/1688527835440-3296f61b-3dbc-4be7-bce6-fcda7f3a2ab4-108208.png)

# 五、端点样式
1. 在绘制连续线段时，canvas提供了多种线段连接点的样式处理
    - 属性为：`ctx.lineJoin = '值'`
        * 尖角：`miter`（默认）
        * 圆角：`round`
        * 斜角：`bevel`

```javascript
const canvas = document.querySelector(".mycanvas");

const ctx = canvas.getContext("2d");

ctx.beginPath();
ctx.moveTo(100, 100);
ctx.lineTo(200, 150);
ctx.lineTo(100, 200);
ctx.lineWidth = 10;
ctx.strokeStyle = "red";
// 设置尖角
ctx.lineJoin = 'miter';
ctx.stroke();

ctx.beginPath();
ctx.moveTo(200, 100);
ctx.lineTo(300, 150);
ctx.lineTo(200, 200);
ctx.lineWidth = 10;
ctx.strokeStyle = "red";
// 设置圆角
ctx.lineJoin = 'round';
ctx.stroke();

ctx.beginPath();
ctx.moveTo(300, 100);
ctx.lineTo(400, 150);
ctx.lineTo(300, 200);
ctx.lineWidth = 10;
ctx.strokeStyle = "red";
// 设置斜角
ctx.lineJoin = 'bevel';
ctx.stroke();
```

效果如图：

![1688528690432-7164502d-a02f-4a3e-9667-d6277af7f3bb.png](./img/rNy4AFfuoNA4jJ2N/1688528690432-7164502d-a02f-4a3e-9667-d6277af7f3bb-128283.png)

2. 在绘制单条线段时，线段两端的样式也可以被控制
    - 属性：`ctx.lineCap = '值'`
        * butt，无，默认
        * round，圆
        * square，方

```javascript
const canvas = document.querySelector(".mycanvas");

const ctx = canvas.getContext("2d");

console.log(ctx);

ctx.beginPath();
ctx.moveTo(100, 100);
ctx.lineTo(200, 100);
ctx.lineWidth = 20;
ctx.strokeStyle = "red";
// 无
ctx.lineCap = "butt";
ctx.stroke();

ctx.beginPath();
ctx.moveTo(100, 150);
ctx.lineTo(200, 150);
ctx.lineWidth = 20;
ctx.strokeStyle = "red";
// 圆
ctx.lineCap = "round";
ctx.stroke();

ctx.beginPath();
ctx.moveTo(100, 200);
ctx.lineTo(200, 200);
ctx.lineWidth = 20;
ctx.strokeStyle = "red";
// 方
ctx.lineCap = "square";
ctx.stroke();
```

效果如图：

![1688529085260-9a7620aa-b813-4dc4-9056-4c42dad8978d.png](./img/rNy4AFfuoNA4jJ2N/1688529085260-9a7620aa-b813-4dc4-9056-4c42dad8978d-970115.png)

+ 注意：
    - square 和 round 会让线段稍稍变长
    - 线段端点样式的颜色会和线段颜色保持一致

# 六、填充
1. 当使用连续线段绘制出几何图形后，还可以对图形进行颜色填充
    - 设置填充颜色：`ctx.fillStyle = '颜色值'`
    - 填充：`ctx.fill()`

```javascript
const canvas = document.querySelector(".mycanvas");

const ctx = canvas.getContext("2d");

ctx.beginPath();
ctx.moveTo(100, 100);
ctx.lineTo(200, 100);
ctx.lineTo(200, 200);
ctx.lineTo(100, 100);
ctx.closePath();

ctx.fillStyle = "pink";
ctx.fill();
```

效果如图：

![1688537175397-744f305d-44c6-496b-b91a-d0bce201ba82.png](./img/rNy4AFfuoNA4jJ2N/1688537175397-744f305d-44c6-496b-b91a-d0bce201ba82-211900.png)

+ 注意：填充时可以不进行路径闭合，填充方法会自动闭合路径后，再进行填充
2. 填充和描边可以同时使用

```javascript
const canvas = document.querySelector(".mycanvas");

const ctx = canvas.getContext("2d");

ctx.beginPath();
ctx.moveTo(100, 100);
ctx.lineTo(200, 100);
ctx.lineTo(200, 200);
ctx.lineTo(100, 100);
ctx.closePath();
// 设置路径样式
ctx.lineWidth = 10;
ctx.strokeStyle = "black";
ctx.stroke();
// 设置填充样式
ctx.fillStyle = "pink";
ctx.fill();
```

效果如图：

![1688538508556-b4d247b8-b799-4168-b184-1c0642ef1402.png](./img/rNy4AFfuoNA4jJ2N/1688538508556-b4d247b8-b799-4168-b184-1c0642ef1402-684750.png)

# 七、canvas 的填充规则 - 非零填充（了解）
### 示例1
+ 绘制一个 "回" 形
+ 注意一个细节 : 
    1. 里面的小正方形我们会按照 顺时针 的方向绘制
    2. 外面的大正方形我们也会按照 顺时针 的方向绘制

```javascript
// 0. 获取到页面上的 canvas 标签元素节点
const canvasEle = document.querySelector('.mycanvas')

// 1. 获取当前这个画布的工具箱
const ctx = canvasEle.getContext('2d')

// 2. 开始绘制里面的小正方形
ctx.moveTo(200, 100);
ctx.lineTo(300, 100);
ctx.lineTo(300, 200);
ctx.lineTo(200, 200);

// 3. 开始绘制外面的大正方形
ctx.moveTo(150, 50);
ctx.lineTo(350, 50);
ctx.lineTo(350, 250);
ctx.lineTo(150, 250);

// 线段样式
ctx.lineWidth = 2;
ctx.strokeStyle = '#000';
ctx.stroke();
```

![1688314624618-2726dd4e-ef1f-43a0-95a7-fb57cdf1e82e.png](./img/rNy4AFfuoNA4jJ2N/1688314624618-2726dd4e-ef1f-43a0-95a7-fb57cdf1e82e-290739.png)

+ 填充后看效果

```javascript
// 4. 填充
ctx.fill()
```

![1688314624782-81e2fb1a-5278-473b-bfc0-836765a44bd5.png](./img/rNy4AFfuoNA4jJ2N/1688314624782-81e2fb1a-5278-473b-bfc0-836765a44bd5-849162.png)

+ 我们发现，两个都被填充了
+ 这是因为，在填充的时候，就是会一次性把所有的内容都会填充好
+ 注意 : 
    - 和是否闭合路径 ( ctx.closePath() ) 没有关系
    - 和里外正方形的绘制先后顺序没有关系

### 示例2
+ 再绘制一个 "回" 形
+ 注意一个细节：
    1. 里面的小正方形我们会按照 逆时针 的方向绘制
    2. 外面的大正方形我们也会按照 顺时针 的方向绘制

```javascript
// 0. 获取到页面上的 canvas 标签元素节点
const canvasEle = document.querySelector('.mycanvas')

// 1. 获取当前这个画布的工具箱
const ctx = canvasEle.getContext('2d')

// 2. 开始绘制里面的小正方形
ctx.moveTo(200, 100);
ctx.lineTo(200, 200);
ctx.lineTo(300, 200);
ctx.lineTo(300, 100);

// 3. 开始绘制外面的大正方形
ctx.moveTo(150, 50);
ctx.lineTo(350, 50);
ctx.lineTo(350, 250);
ctx.lineTo(150, 250);

// 线段样式
ctx.lineWidth = 2;
ctx.strokeStyle = '#000';
ctx.stroke();
```

![1688314624870-6df79c94-cc8c-4b44-8cc0-c11826c0f206.png](./img/rNy4AFfuoNA4jJ2N/1688314624870-6df79c94-cc8c-4b44-8cc0-c11826c0f206-394640.png)

填充后看效果：

```javascript
// 4. 填充
ctx.fill()
```

![1688314624960-d60d114a-7f98-4d9a-9e67-f39a475374cd.png](./img/rNy4AFfuoNA4jJ2N/1688314624960-d60d114a-7f98-4d9a-9e67-f39a475374cd-123279.png)

+ 此时发现，和刚才填充出来的结果不一样了
+ 可以得出结论：填充的区域和线段绘制时的 顺时针 逆时针 方向有关系！

### 非零填充
1. 其实我们的填充和顺时针逆时针有关系，但不是简单的顺逆时针的问题
2. 非零填充的概念 : 
    - <font style="color:#DF2A3F;">从任何一个区域向画布最外层移动</font>
    - <font style="color:#DF2A3F;">按照经历最少的边数量计算</font>
    - <font style="color:#DF2A3F;">其中经历的顺时针边，记录为 +1</font>
    - <font style="color:#DF2A3F;">经历的逆时针边，记录为 -1</font>
    - <font style="color:#DF2A3F;">只要最终总和</font><font style="color:#DF2A3F;">不为 零</font><font style="color:#DF2A3F;">，那么该区域</font><font style="color:#DF2A3F;">填充</font>
    - <font style="color:#DF2A3F;">如果最终总和为 零，那么该区域不填充</font>

### 示例3：
+ 这次我们绘制一个稍微复杂一些的图形

![1688314625005-b0bccd50-1199-4461-8be0-6c161711fa0d.png](./img/rNy4AFfuoNA4jJ2N/1688314625005-b0bccd50-1199-4461-8be0-6c161711fa0d-887860.png)

+ 这是两个矩形对接在一起, 一个是顺时针绘制, 一个是逆时针绘制
+ 我们来分析一下看看
+ 首先, 最左侧封闭图形区域

![1688314625043-a6724937-09e8-4916-9441-acdaf2a61dc5.png](./img/rNy4AFfuoNA4jJ2N/1688314625043-a6724937-09e8-4916-9441-acdaf2a61dc5-203385.png)

    - 如果走最短的路线出来的话，会经历一条顺时针的边
    - 记录为 +1
    - 最终为 +1
    - 所以该区域会被填充
+ 然后, 最右侧封闭图形

![1688314625187-def60f8e-e1a8-47bb-b0f4-dd674bcd7db1.png](./img/rNy4AFfuoNA4jJ2N/1688314625187-def60f8e-e1a8-47bb-b0f4-dd674bcd7db1-309989.png)

    - 经历最短路线出来的话，会经历一条逆时针的边
    - 记录为 -1
    - 最终为 -1
    - 所以该区域会被填充
+ 最后, 中间的封闭图形

![1688314625231-1ff5c46c-301d-486b-9e98-9627ee75472f.png](./img/rNy4AFfuoNA4jJ2N/1688314625231-1ff5c46c-301d-486b-9e98-9627ee75472f-832583.png)

    - 经历最短路线出来的话，必然会经历一条顺时针的边 和 一条逆时针的边
    - 顺时针记录为 +1
    - 逆时针记录为 -1
    - 合计为 0
    - 所以该区域不会被填充
+ 实际测试

```javascript
// 0. 获取到页面上的 canvas 标签元素节点
const canvasEle = document.querySelector('.mycanvas')

// 1. 获取当前这个画布的工具箱
const ctx = canvasEle.getContext('2d')

// 2. 开始绘制里面的小正方形
// 顺时针绘制
ctx.moveTo(100, 50);
ctx.lineTo(200, 50);
ctx.lineTo(200, 150);
ctx.lineTo(100, 150);
ctx.lineTo(100, 50);
// 逆时针绘制
ctx.moveTo(150, 100);
ctx.lineTo(150, 200);
ctx.lineTo(250, 200);
ctx.lineTo(250, 100);
ctx.lineTo(150, 100);

ctx.lineWidth = 2;
ctx.strokeStyle = '#000';
ctx.stroke();

// 4. 填充
ctx.fill()
```

+ 效果如图：

![1688314625423-d23ea661-f2a7-44b0-abdc-a19157572383.png](./img/rNy4AFfuoNA4jJ2N/1688314625423-d23ea661-f2a7-44b0-abdc-a19157572383-603645.png)

# 八、绘制矩形
+ 绘制矩形的方法有三个：
1. 矩形路径：
    - 语法：`ctx.rect( 矩形起点 x 轴坐标, 矩形起点 y 轴坐标, 矩形宽度, 矩形高度 )`
    - 如：`ctx.rect(100, 100, 100, 100)`
    - 表示在坐标 100，100 的位置绘制一个 100*100 的矩形路径，默认无填充无描边
    - 可通过`ctx.stroke()`描边，通过`ctx.fill()`填充
    - 可通过`lineWidth`，`strokeStyle`，`fillStyle`属性，分别设置描边宽度，描边色，填充色
2. 描边矩形：
    - 语法：`ctx.strokeRect( 矩形起点 x 轴坐标, 矩形起点 y 轴坐标, 矩形宽度, 矩形高度 )`
    - 如：`ctx.strokeRect(300, 100, 100, 100)`
    - 表示在坐标 300，100 的位置绘制一个 100*100 的描边矩形，无填充
    - 可通过`lineWidth`，`strokeStyle`属性，设置描边宽度，描边色
3. 填充矩形：
    - 语法：`ctx.fillRect( 矩形起点 x 轴坐标, 矩形起点 y 轴坐标, 矩形宽度, 矩形高度 )`
    - 如：`ctx.fillRect(500, 100, 100, 100)`
    - 表示在坐标 500，100 的位置绘制一个 100*100 的填充矩形，无描边
    - 可通过`fillStyle`属性，设置填充色

```javascript
// 0. 获取到页面上的 canvas 标签元素节点
const canvasEle = document.querySelector('.mycanvas')

// 1. 获取当前这个画布的工具箱
const ctx = canvasEle.getContext('2d')

// 2. 绘制矩形路径
ctx.rect(100, 100, 100, 100);

// 3. 绘制描边矩形
ctx.strokeRect(300, 100, 100, 100);

// 4. 绘制填充矩形
ctx.fillRect(500, 100, 100, 100);
```

效果如图：

![1688607527429-8d54800c-8e1f-45ec-b622-6adf7bb06ea1.png](./img/rNy4AFfuoNA4jJ2N/1688607527429-8d54800c-8e1f-45ec-b622-6adf7bb06ea1-216412.png)

# 九、绘制圆形
1. 什么是圆：
    - 圆 就是从一个点出发，按照半径，画弧线，当弧线饶了一圈回到原点的时候，就是一个圆形。
    - 在canvas内，绘制圆形，其实就是在绘制弧线
2. 什么是弧度
    - 这是一个圆，圆心为 o，半径为 r

![1688547601988-ff5c8f07-0c5b-4380-b93a-05d134c99bd0.png](./img/rNy4AFfuoNA4jJ2N/1688547601988-ff5c8f07-0c5b-4380-b93a-05d134c99bd0-810956.png)

    - 以圆心 o 做坐标轴，x 轴正方向上和圆周的交点为弧度起点

![1688547601925-7fc575b3-0c15-4283-8fa3-744864e80100.png](./img/rNy4AFfuoNA4jJ2N/1688547601925-7fc575b3-0c15-4283-8fa3-744864e80100-312407.png)

    - 在圆周上，从弧度起点，顺着圆周移动，移动的距离成为弧长，当弧长和半径一样时
    - 这段弧长所对应的圆心角是 1 弧度

![1688547601908-1ef9add3-1a88-4810-b6f6-862b93374c89.png](./img/rNy4AFfuoNA4jJ2N/1688547601908-1ef9add3-1a88-4810-b6f6-862b93374c89-811721.png)

    - 根据圆周公式 : 周长 = 2 * π * r
    - 所以：
        * 一个圆周是 : 2 * π
        * 半个圆周是 : π
        * 四分之一圆周是 : π / 2
3. 了解了什么是弧度，接下来就可以开始绘制弧线了，绘制弧线有两种方式：圆弧，椭圆弧
    - 圆弧
        * 语法：`ctx.arc( x, y, r, startAngle, endAngle, counterclockwise )`
            + x：圆心的 x 轴坐标
            + y：圆心的 y 周坐标
            + r：圆的半径
            + startAngle：绘制弧线的起点弧度
            + endAngle：绘制弧线的终点弧度
            + counterclockwise：方向，false 为顺时针(默认)，true 为逆时针
        * 如：`ctx.arc( 150, 150, 100, 0, 1, false )`
        * 表示绘制一个圆心在坐标 150，150，半径100，从 0 顺时针 到 1 的弧线路径，默认无填充无描边

```javascript
// 0. 获取到页面上的 canvas 标签元素节点
const canvasEle = document.querySelector('.mycanvas')

// 1. 获取当前这个画布的工具箱
const ctx = canvasEle.getContext('2d')

// 2. 绘制圆弧
ctx.arc( 150, 150, 100, 0, Math.PI, false )

// 3. 描边
ctx.lineWidth = 2;
ctx.strokeStyle = "red";
ctx.stroke();
```

![1688608850522-d99d22cf-d3c2-478c-9a05-55bfb09d6fb4.png](./img/rNy4AFfuoNA4jJ2N/1688608850522-d99d22cf-d3c2-478c-9a05-55bfb09d6fb4-840157.png)

    - 椭圆弧
        * 语法：`ctx.ellipse( x, y, radiusX, radiusY, rotation, startAngle, endAngle, antiClockwise )`
            + x：椭圆中心点的 x 轴坐标
            + y：椭圆中心点的 y 轴坐标
            + radiusX：椭圆在 x 轴方向上的半径
            + radiusY：椭圆在 y 轴方向上的半径
            + rotation：旋转弧度，指讲该椭圆进行旋转
            + startAngle：弧线开始弧度
            + endAngle：弧线结束弧度
            + antiClockwise：方向，false 表示逆时针绘制(默认)，true 表示顺时针绘制
        * 如：`ctx.ellipse( 300, 150, 200, 100, 0, 0, Math.PI * 2, false )`
        * 表示绘制一个圆心在坐标 350，150，x轴半径200，y轴半径为100，不旋转，从 0 顺时针 到 Math.PI * 2 的弧线路径，默认无填充无描边

```javascript
// 0. 获取到页面上的 canvas 标签元素节点
const canvasEle = document.querySelector('.mycanvas')

// 1. 获取当前这个画布的工具箱
const ctx = canvasEle.getContext('2d')

// 2. 绘制椭圆弧
ctx.ellipse( 300, 150, 200, 100, 0, 0, Math.PI * 2, false )

// 3. 描边
ctx.lineWidth = 2;
ctx.stroke();
```

![1688547602442-9f6c2538-9c23-4d64-8c53-3ca400bde06e.png](./img/rNy4AFfuoNA4jJ2N/1688547602442-9f6c2538-9c23-4d64-8c53-3ca400bde06e-174814.png)

        * 这样一个椭圆就出来了，解释一下这些内容的意义

![1688547602586-1b22b7cc-2db0-4371-af37-285dfcdc14d2.png](./img/rNy4AFfuoNA4jJ2N/1688547602586-1b22b7cc-2db0-4371-af37-285dfcdc14d2-992696.png)

        * 旋转弧度，就是在现在的基础上，让整个图形进行旋转

```javascript
ctx.ellipse( 300, 150, 200, 100, Math.PI / 2, 0, Math.PI * 2, false )
```

![1688609367849-a1b2b4ab-622a-45b2-a5b3-b1e7cfb9525e.png](./img/rNy4AFfuoNA4jJ2N/1688609367849-a1b2b4ab-622a-45b2-a5b3-b1e7cfb9525e-554934.png)

# 十、擦除画布
1. 就像画画时的橡皮擦，擦除掉指定区域的内容
2. 语法：`工具箱.clearRect( 矩形起点 x 轴坐标, 矩形起点 y 轴坐标, 矩形宽度, 矩形高度 )`
    - 如：`工具箱.clearRect( 150, 150, 30, 30 )`
    - 表示从坐标 150, 150 位置开始，擦除一块 30 * 30 的区域

![1688547601386-5e1f3aa0-0fd3-4ca8-a050-8894460231ea.png](./img/rNy4AFfuoNA4jJ2N/1688547601386-5e1f3aa0-0fd3-4ca8-a050-8894460231ea-187890.png)

3. 注意：
    - clearRect默认只能擦除填充和描边，并不能擦除路径
    - canvas中的绘制方法（如stroke，fill），会以“上一次 beginPath 之后的所有路径为基础进行绘制
    - 如果没有使用beginPath()方法，上一次描述的路径没有被清除，这一次进行描边等操作还会绘制出之前的路径，表现出一种类似没有擦除的状态。
    - 所以为了**彻底擦除，在使用了clearRect后，一般都会再执行一次beginPath方法**

# 十一、绘制文字
1. 在canvas内可以直接绘制文字，不需要通过线段一笔一划的写出来
    - 描边文字（空心）：
        * 语法：`ctx.strokeText("文字内容", x 坐标, y 坐标);`
    - 填充文字（实心）：
        * 语法：`ctx.fillText("文字内容", x 坐标, y 坐标);`

```javascript
  // 0. 获取到页面上的 canvas 标签元素节点
  const canvasEle = document.querySelector('.mycanvas')

  // 1. 获取当前这个画布的工具箱
  const ctx = canvasEle.getContext('2d')

  // 2. 绘制文字
  ctx.fillText("测试", 100, 100);
  ctx.strokeText("大前端", 100, 200);
```

![1688615147887-2328e92c-7099-481b-b515-3a1d700c0f36.png](./img/rNy4AFfuoNA4jJ2N/1688615147887-2328e92c-7099-481b-b515-3a1d700c0f36-924080.png)

2. 文字样式修饰
    - 字体大小：`ctx.font = '字体大小 字体'`
    - 文字水平对齐：`ctx.textAlign = 'left | center | right';`
    - 文字垂直对齐：`ctx.textBaseline = 'top | middle | bottom';`
    - 注意：**文字的样式修饰需要在绘制之前设置**

```javascript
// 0. 获取到页面上的 canvas 标签元素节点
const canvasEle = document.querySelector('.mycanvas')

// 1. 获取当前这个画布的工具箱
const ctx = canvasEle.getContext('2d')

// 2. 设置文字样式
ctx.font = "50px 黑体";
ctx.fillText("测试", 100, 100);


```

![1688615743179-72d83f91-777d-47f6-8ea5-c16024a53468.png](./img/rNy4AFfuoNA4jJ2N/1688615743179-72d83f91-777d-47f6-8ea5-c16024a53468-273685.png)

| 水平对齐：ctx.textAlign | 垂直对齐：ctx.textBaseline |
| --- | --- |
| start：文本在指定的位置开始<br/>end：文本在指定的位置结束<br/>center：居中对齐<br/>left：左对齐<br/>right：右对齐 | alphabetic：默认，文本基线是普通的字母基线<br/>top：文本基线是 em 方框的顶端<br/>hanging：文本基线是悬挂基线<br/>middle：文本基线是 em 方框的正中<br/>ideographic：文本基线是 em 基线<br/>bottom：文本基线是 em 方框的底端 |
| ![1688622665608-142ed967-01cf-4f64-918e-4fa9d0c8bb3f.png](./img/rNy4AFfuoNA4jJ2N/1688622665608-142ed967-01cf-4f64-918e-4fa9d0c8bb3f-047073.png) | ![1688622672460-6e9482b9-7346-43a6-9e0b-e9a3ed84b0ef.png](./img/rNy4AFfuoNA4jJ2N/1688622672460-6e9482b9-7346-43a6-9e0b-e9a3ed84b0ef-189603.png) |


3. 获取文本信息
    - 语法：`ctx.measureText("文本")`
    - 获取文本宽度：`ctx.measureText("前端").width`
    - 用于给文本添加下划线，或边框线等操作

# 十二、阴影
1. 在canvas中，还可以对路径添加阴影效果（文字也可以有阴影）
    - 阴影 x 轴偏移：`ctx.shadowOffsetX = number;`
    - 阴影 y 轴偏移：`ctx.shadowOffsetY = number;`
    - 模糊大小：`ctx.shadowBlur = number;`
    - 阴影颜色：`ctx.shadowColor = '颜色值';`

```javascript
// 0. 获取到页面上的 canvas 标签元素节点
const canvasEle = document.querySelector('.mycanvas');

// 1. 获取当前这个画布的工具箱
const ctx = canvasEle.getContext('2d');

// 2. 设置阴影效果
ctx.shadowOffsetX = 30;
ctx.shadowOffsetY = 10;
ctx.shadowBlur = 2;
ctx.shadowColor = "#aaa";

// 3. 绘制文本
ctx.font = "50px 黑体";
ctx.strokeText("测试", 100, 50);
ctx.fillText("测试", 300, 50);

// 4. 绘制矩形
ctx.strokeRect( 100, 80, 100, 50);
ctx.fillRect( 300, 80, 100, 50);

// 5. 绘制圆
ctx.arc( 150, 200, 50, 0, 2 * Math.PI);
ctx.stroke();
ctx.beginPath();
ctx.arc( 350, 200, 50, 0, 2 * Math.PI);
ctx.fill();
```
+ 如需不同的阴影效果，每次绘制前可以重新配置

# 十三、绘制虚线
+ 语法：`工具箱.setLineDash([ 第一段长度, 第二段长度, ... ])`
+ 如：`ctx.setLineDash([5, 10])`
+ 表示绘制出的虚线为：实5，虚10，实5，虚10，......

```javascript
// 0. 获取到页面上的 canvas 标签元素节点
const canvasEle = document.querySelector('.mycanvas');

// 1. 获取当前这个画布的工具箱
const ctx = canvasEle.getContext('2d');
console.log(ctx);

// 2. 设置虚线方案：实线5px 和 虚线10px 重复出现
ctx.setLineDash([ 5, 10 ]);

// 3. 绘制线段
ctx.moveTo( 100, 50 );
ctx.lineTo( 400, 50 );
ctx.strokeStyle = '#000';
ctx.lineWidth = 2;
ctx.stroke();

// 4. 绘制文本
ctx.font = "100px 黑体";
ctx.strokeText("测试", 100, 200);
```

注意：**填充无法设置虚线效果**

# 十四、总结
1. 创建一个画笔对象（获取工具箱）
    - `const ctx = canvasElement.getContext('2d');`
2. 常用属性
    - 线条粗细：`ctx.lineWidth = number;`
    - 描边色：`ctx.strokeStyle = '颜色值';`
    - 端点样式：`ctx.lineCap = 'butt | round | square';`
    - 接洽点样式：`ctx.lineJoin = 'miter | bevel | round';`
    - 填充色：`ctx.fillStyle = '颜色值';`
    - 字体大小：`ctx.font = '字体大小 字体';`
    - 文字水平对齐：`ctx.textAlign = 'left | center | right';`
    - 文字垂直对齐：`ctx.textBaseline = 'top | middle | bottom';`
    - 阴影 x 轴偏移：`ctx.shadowOffsetX = number;`
    - 阴影 y 轴偏移：`ctx.shadowOffsetY = number;`
    - 模糊大小：`ctx.shadowBlur = number;`
    - 阴影颜色：`ctx.shadowColor = '颜色值';`
3. 常用方法
    - 下次绘制开启新路径（弃用已存在路径）：`ctx.beginPath();`
    - 开始绘制线段的起点：`ctx.moveTo(x, y);`
    - 连线到：`ctx.lineTo(x, y);`
    - 闭合路径：`ctx.closePath();`
    - 描边：`ctx.stroke();`
    - 填充：`ctx.fill();`
    - 矩形路径：`ctx.rect(x, y, w, h);`
    - 描边矩形：`ctx.strokeRect(x, y, w, h);`
    - 填充矩形：`ctx.fillRect(x, y, w, h);`
    - 圆弧路径：`ctx.arc(cx, cy, r, start, end, false);`
    - 椭圆弧路径：`ctx.ellipse( cx, cy, xr, yr, rotate, start, end, false );`
    - 擦除：`ctx.clearRect(x, y, w, h);`
    - 填充文字：`ctx.fillText(string, x, y);`
    - 描边文字：`ctx.strokeText(string, x, y);`
    - 获取文字信息：`ctx.measureText('测试');`
    - 设置虚线：`setLineDash([线段1宽度, 线段2宽度, ...])`
