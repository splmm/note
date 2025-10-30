---
title: canvas - 下
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-16 20:00:00
---
# 34：canvas - 下

# 一、转换
+ 在canvas内，也可以像css一样有一些类似于css2D转换的效果
1. 位移
    - 语法：`ctx.translate(x, y)`
    - 注意：一定要在绘制（描边或填充）之前，进行位移
2. 缩放
    - 语法：`ctx.scale(x, y)`
    - 注意：一定要在绘制（描边或填充）之前，进行缩放
3. 旋转
    - 语法：`ctx.rotate(弧度值)`
        * 角度转弧度公式：`弧度 = Math.PI/180*角度`
    - 注意：一定要在绘制（描边或填充）之前，进行旋转
+ 转换的中心都是画布的 0，0 点，可以配合 位移 修改旋转或缩放的中心

![1688912980718-e285d4bf-359a-483d-99f1-ca27988dc1b8.png](./img/k_IlbS3L5K1yWmgE/1688912980718-e285d4bf-359a-483d-99f1-ca27988dc1b8-249328.png)

4. 注意：
    - canvas所有的转换操作都不只是在操作某个形状，而是对**整个画布进行转换**
    - 如果需要对多个形状进行不同的转换，在每次绘制之前都需要先保存画笔状态，绘制之后重置画笔状态
        * 保存画笔状态：`ctx.save();`
            + 一般存在于转换之前
        * 重置画笔状态：`ctx.restore();`
            + 一般存在于转换之后

```javascript
// 绘制两个矩形
ctx.strokeRect(100, 100, 100, 100);
ctx.strokeRect(300, 100, 100, 100);
```

![1688912152450-d16ffd5a-6f35-4a42-8899-0b9a6b6166a3.png](./img/k_IlbS3L5K1yWmgE/1688912152450-d16ffd5a-6f35-4a42-8899-0b9a6b6166a3-286617.png)

```javascript
// 对画布进行旋转
ctx.rotate(0.5);
ctx.strokeRect(100, 100, 100, 100);
ctx.strokeRect(300, 100, 100, 100);
```

![1688912216377-d71bbdfc-f622-4f63-b920-26370a57be1a.png](./img/k_IlbS3L5K1yWmgE/1688912216377-d71bbdfc-f622-4f63-b920-26370a57be1a-929800.png)

```javascript
// 将 旋转和其中一个矩形绘制 存储为一次记录
ctx.save();
ctx.rotate(0.5);
ctx.strokeRect(100, 100, 100, 100);
ctx.restore();

ctx.strokeRect(300, 100, 100, 100);
```

![1688912289995-afb06b12-06ba-4d75-9315-26f46011e05f.png](./img/k_IlbS3L5K1yWmgE/1688912289995-afb06b12-06ba-4d75-9315-26f46011e05f-368524.png)

# 二、渐变色
1. canvas的渐变色，就是提前配置好渐变色方案（从颜色1向颜色2过渡），然后将渐变色方案，设置给填充样式即可
2. 渐变形式
    - 线性渐变
        * 创建渐变：`const lg = ctx.createLinearGradient(起点x坐标, 起点y坐标, 终点x坐标, 终点y坐标)`
            + 指定渐变范围
        * 添加渐变色：`lg.addColorStop(0, 'red')`
            + 向指定位置添加颜色，0表示开始坐标，1表示结束坐标，中间部分会自动填充渐变色

```javascript
  const canvas = document.querySelector(".mycanvas");

  canvas.width = 800;
  canvas.height = 400;

  const ctx = canvas.getContext("2d");

  const lg = ctx.createLinearGradient(0, 0, 800, 400);
  
  lg.addColorStop(0, 'red');
  lg.addColorStop(1, 'green');

  ctx.fillStyle = lg;
  ctx.fillRect(0, 0, 800, 400);
```

![1688914058322-229bb0f5-9341-472b-a826-510da597bf94.png](./img/k_IlbS3L5K1yWmgE/1688914058322-229bb0f5-9341-472b-a826-510da597bf94-243799.png)

    - 径向渐变
        * 创建渐变：`const lg = ctx.createRadialGradient(x1, y1, r1, x2, y2, r2);`
            + <font style="color:#333333;">x1：起始圆圆心 x 轴坐标</font>
            + <font style="color:#333333;">y1：起始圆圆心 y 轴坐标</font>
            + <font style="color:#333333;">r1：起始圆半径</font>
            + <font style="color:#333333;">x2：终止圆圆心 x 轴坐标</font>
            + <font style="color:#333333;">y2：终止圆圆心 y 轴坐标</font>
            + <font style="color:#333333;">r2：终止圆半径</font>
        * 添加渐变色：`lg.addColorStop(0, 'red')`
            + 同线性渐变

```javascript
const canvas = document.querySelector(".mycanvas");

canvas.width = 800;
canvas.height = 400;

const ctx = canvas.getContext("2d");

const lg = ctx.createRadialGradient(200, 200, 50, 200, 200, 200);

lg.addColorStop(0, 'red');
lg.addColorStop(1, 'green');

ctx.fillStyle = lg;
ctx.fillRect(0, 0, 800, 400);
```

![1688914439313-f0cb19ae-8009-4979-8a26-6fb3bdc73d73.png](./img/k_IlbS3L5K1yWmgE/1688914439313-f0cb19ae-8009-4979-8a26-6fb3bdc73d73-128856.png)

3. 多区域不同渐变
    - 本质为配置多套渐变方案，绘制到不同的形状

```javascript
const canvas = document.querySelector(".mycanvas");

canvas.width = 800;
canvas.height = 400;

const ctx = canvas.getContext("2d");

const rg = ctx.createRadialGradient(200, 200, 50, 200, 200, 200);
rg.addColorStop(0, 'red');
rg.addColorStop(1, 'green');

ctx.fillStyle = rg;
ctx.fillRect(0, 0, 400, 400);

const lg = ctx.createLinearGradient(400, 0, 800, 0);
lg.addColorStop(0, 'blue');
lg.addColorStop(1, 'yellow');
ctx.fillStyle = lg;
ctx.fillRect(400, 0, 400, 400);
```

![1688915008415-957d0f07-3a95-48fd-a5cd-4186c3c47ea2.png](./img/k_IlbS3L5K1yWmgE/1688915008415-957d0f07-3a95-48fd-a5cd-4186c3c47ea2-890485.png)

# 三、贝塞尔曲线
+ 贝塞尔曲线（Bezier curve）是计算机图形学中相当重要的参数曲线，它通过一个方程来描述一条曲线，根据方程的最高阶数，又分为线性贝赛尔曲线，二次贝塞尔曲线、三次贝塞尔曲线和更高阶的贝塞尔曲线。
    - 贝塞尔曲线需要提供几个点的参数，首先是 曲线的起点和终点
    - 如果控制点数量为 0，我们称之为线性贝塞尔；
    - 控制点数量为 1，则为二阶贝塞尔曲线；
    - 控制点数量为 2，则为三阶贝塞尔曲线，依此类推。
1. 二阶贝塞尔曲线
    - 其实就是由 三个点 绘制成两个直线
    - 然后同时从每条直线的起点开始，向终点移动，按比例拿到点。然后将这些点再连接，产生 n - 1 条直线。
    - 就这样，我们继续同样的操作的，直到变成一条直线，然后再按比例取到一个点，这个点就是曲线经过的点。
    - 当我们比例一点点变大（从 0 到 1），就拿到了曲线中间的所有点，最终绘制出完整的曲线。

![1689010509569-0a0a06fa-5c4b-497f-b67c-bf5cc37c8a68.gif](./img/k_IlbS3L5K1yWmgE/1689010509569-0a0a06fa-5c4b-497f-b67c-bf5cc37c8a68-415868.gif)

2. 再来看看三阶贝塞尔曲线
    - 和二阶贝塞尔曲线是一个道理，只不过控制点数量变成了两个

![1689010509565-02b3d8f8-e762-47c1-b9f7-1c0d1e54a9a3.gif](./img/k_IlbS3L5K1yWmgE/1689010509565-02b3d8f8-e762-47c1-b9f7-1c0d1e54a9a3-468577.gif)

3. 在canvas中不需要我们手动计算这么多点，canvas直接提供了相关的API
    - 二阶贝塞尔曲线：`ctx.quadraticCurveTo(p1x, p1y, p2x, p2y)`
    - 三阶贝塞尔曲线：`ctx.bezierCurveTo(p1x, p1y, p2x, p2y, p3x, p3y)`
    - 在此之前需要先使用 moveTo 确定 p0 的位置

**二阶**

```javascript
const cvs = document.querySelector(".cvs");
cvs.width = 400;
cvs.height = 400;

const ctx = cvs.getContext("2d");

ctx.beginPath();
ctx.moveTo(100, 100);
ctx.quadraticCurveTo(300, 200, 200, 300);
ctx.stroke();
```

![1689011555165-4be5506d-9a47-46f7-90bd-503bdeeaa1b2.png](./img/k_IlbS3L5K1yWmgE/1689011555165-4be5506d-9a47-46f7-90bd-503bdeeaa1b2-166182.png)

**三阶**

```javascript
const cvs = document.querySelector(".cvs");
cvs.width = 400;
cvs.height = 400;

const ctx = cvs.getContext("2d");

ctx.beginPath();
ctx.moveTo(100, 100);
ctx.bezierCurveTo(60, 80, 150, 30, 170, 150);
ctx.stroke();
```

![1689011610731-ce6dc189-3538-4ae3-91f0-70043d992ed4.png](./img/k_IlbS3L5K1yWmgE/1689011610731-ce6dc189-3538-4ae3-91f0-70043d992ed4-619886.png)

**多阶**

```javascript
ctx.beginPath();
ctx.moveTo(75, 25);
ctx.quadraticCurveTo(25, 25, 25, 62.5);
ctx.quadraticCurveTo(25, 100, 50, 100);
ctx.quadraticCurveTo(50, 120, 30, 125);
ctx.quadraticCurveTo(60, 120, 65, 100);
ctx.quadraticCurveTo(125, 100, 125, 62.5);
ctx.quadraticCurveTo(125, 25, 75, 25);
ctx.stroke();
```

```javascript
ctx.moveTo(75, 40);
ctx.bezierCurveTo(75, 37, 70, 25, 50, 25);
ctx.bezierCurveTo(20, 25, 20, 62.5, 20, 62.5);
ctx.bezierCurveTo(20, 80, 40, 102, 75, 120);
ctx.bezierCurveTo(110, 102, 130, 80, 130, 62.5);
ctx.bezierCurveTo(130, 62.5, 130, 25, 100, 25);
ctx.bezierCurveTo(85, 25, 75, 37, 75, 40);
ctx.fill();
```



# 四、绘制图片
1. 创建图片（非canvas操作）
    - 创建图片对象：`const img = new Image();`
    - 设置资源地址：`img.src = "图片地址"`
    - 资源加载完成：`img.onload = function(){ / * 图片加载完成 */  }`
2. 将图片绘制到canvas
    - 三个参数：`gd.drawImage(图片对象, x, y)`
        * 从画布的 x，y 坐标开始绘制

```javascript
const canvas = document.querySelector(".mycanvas");

canvas.width = 800;
canvas.height = 400;

const ctx = canvas.getContext("2d");

const img = new Image();
img.src = "../1.png";

img.onload = function(){
  ctx.drawImage(this, 0, 100);
}
```

![1688915909504-dc331fce-259d-4715-ad81-89c6fb4eb997.png](./img/k_IlbS3L5K1yWmgE/1688915909504-dc331fce-259d-4715-ad81-89c6fb4eb997-477729.png)

    - 五个参数：`gd.drawImage(图片对象, x, y, w, h)`
        * 从画布的 x，y 坐标开始绘制，绘制到 宽w，高h 的区域

```javascript
const canvas = document.querySelector(".mycanvas");

canvas.width = 800;
canvas.height = 400;

const ctx = canvas.getContext("2d");

const img = new Image();
img.src = "../1.png";

img.onload = function(){
  ctx.drawImage(this, 0, 100, 200, 200);
}
```

![1688915942823-93682cf3-6fed-46eb-89ed-d2f20414c170.png](./img/k_IlbS3L5K1yWmgE/1688915942823-93682cf3-6fed-46eb-89ed-d2f20414c170-690417.png)

    - 九个参数：`gd.drawImage(图片对象, sx, sy, sw, sh, dx, dy, dw, dh)`
        * s = source 原图    位置 宽高
        * d = destination 目标（画布）画在哪，画多大

```javascript
const canvas = document.querySelector(".mycanvas");

canvas.width = 800;
canvas.height = 400;

const ctx = canvas.getContext("2d");

const img = new Image();
img.src = "../1.png";
// 图片资源宽高
const imgW = 128;
const imgH = 194;
img.width = 128;
img.height = 194;

img.onload = function(){
  ctx.drawImage(this, 0, 0, imgW/4, imgH/4, 0, 100, imgW/4, imgH/4);
}
```

![1688916025413-f803056a-d7ac-4954-bc6d-e6d61020344a.png](./img/k_IlbS3L5K1yWmgE/1688916025413-f803056a-d7ac-4954-bc6d-e6d61020344a-470262.png)

# 五、事件
1. canvas内没有事件系统，只能通过给canvas元素添加事件，配合事件对象，手动检测事件区域
2. 矩形检测公式：
    - `点击x > 矩形x && 点击x < 矩形x + 矩形w && 点击y > 矩形y && 点击y < 矩形y + 矩形h`
3. 圆形检测公式：
    - 利用勾股定理：a^2 + b^2 = c^2
    - a = 圆心x - 点击x
    - b = 圆心y - 点击y
    - c = Math.sqrt( a * a + b * b )
    - 若 c < r ，则在圆形区域内
4. 自动检测
    - `ctx.isPointInPath(x, y)`
    - 返回值：布尔值，表示指定坐标是否在一个路径范围内

# 六、导出（了解）
```javascript
download.onclick = function(){
  // 将canvas数据转成base64数据
  const base64 = canvas.toDataURL("image/png");
  // 将base64数据解码
  const strBase64 = atob( base64.split(",")[1] );
  // 创建utf-8原始数据（长度为base64解码后的字符长度）
  const utf_8 = new Uint8Array(strBase64.length);
  // 将base64解码后的数据转成Unicode编码后，存入utf-8数组
  for(let n=0;n<strBase64.length;n++){
    utf_8[n] = strBase64.charCodeAt(n)
  }
  // 根据utf-8数据，创建文件对象
  const file = new File([utf_8], "测试图片", {type:"image/png"});
  // 根据文件对象创建url字符
  const href = URL.createObjectURL(file);
  // 准备a标签，用于下载文件
  const a = document.createElement("a");
  a.href = href;
  a.download = "测试图片";
  document.body.appendChild(a);
  a.click();
  // 删除a标签
  a.remove();
  // 释放指向文件资源的url字符
  URL.revokeObjectURL(href);
}
```

# 七、总结
+ 位移：`ctx.translate(x, y)`
+ 旋转：`ctx.rotate(弧度值)`
+ 缩放：`ctx.scale(x, y)`
+ 保存画笔状态：`ctx.save()`
+ 重置画笔状态：`ctx.restore()`
+ 创建线性渐变：`const lg = ctx.createLinearGradient(起点x坐标, 起点y坐标, 终点x坐标, 终点y坐标)`
+ 创建径向渐变：`const lg = ctx.createRadialGradient(x1, y1, r1, x2, y2, r2);`
+ 添加渐变色：`lg.addColorStop(0, 'red')`
+ 绘制图片：`gd.drawImage(图片对象, x, y)`
+ 检测指定坐标是否在某个路径范围内：`ctx.isPointInPath(x, y)`

# 八、拓展 - <font style="color:rgb(34, 34, 38);">requestAnimationFrame</font>
[requestAnimationFrame](https://blog.csdn.net/weixin_41636483/article/details/112622364?spm=1001.2014.3001.5501)
