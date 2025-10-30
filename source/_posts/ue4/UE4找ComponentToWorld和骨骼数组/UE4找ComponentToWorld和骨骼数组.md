---
title: UE4找ComponentToWorld和骨骼数组
date: 2021-06-21 02:38:00
categories: 
    - windows
tags:
    - UE4
    - 游戏开发
    - 内存分析
cover: /usr/uploads/2021/06/1089640715.png
---

## 为什么要找骨骼基址？

找骨骼基址是为了实现绘制骨架和骨骼自瞄，比如瞄准头部骨骼、胸部骨骼。

## 找骨骼基址

对自己的 Actor 的地址进行结构分析，防止分析错地址最好直接用 APawn，然后根据特征去查找。

ComponentToWorld 是 3x3 结构，最后一行数值不会动，最后一行数值一般是3个固定的数值和 0.00。例如：
- 1.00 1.00 1.00 0.00 
- 2.50 2.50 2.50 0.00

### ComponentToWorld位置

根据特征，我们找到了 ComponentToWorld，得到偏移为:
`Actor.base -> 0x378 -> 0x180`，其中 0x378 偏移是 Mesh 的偏移。

![ComponentToWorld位置](/usr/uploads/2021/06/1089640715.png)

### 骨骼数组位置

ComponentToWorld 和骨骼数组都在 Mesh 下面。找到 ComponentToWorld 后，很容易找到骨骼数组。骨骼数组在内存中表现很规律，偏移为:
`Actor.base -> 0x378 -> 0x698`

![骨骼数组位置](/usr/uploads/2021/06/1688538075.png)

### 特殊情况

有时候骨骼数组里的数值并不会发生变动，但仍然保持规律。如下图所示(GS:GO)：

![CSGO骨骼数组](/usr/uploads/2021/06/1213761717.png)

## 实际应用

最后可以利用以下要素绘制骨架：
- ComponentToWorld
- 骨骼数组
- 摄像机矩阵
- 具体关节的实际偏移


