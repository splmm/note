---
title: UE4找Count基址
date: 2021-06-21 02:21:20
categories: windows
urlname: 36
cover: /usr/uploads/2021/06/3564924053.png
tags:
- UE4
- 游戏开发
- 内存分析
---

## 什么是Count？

Count 就是 UE4 引擎开发的游戏在运行过程中动态创建的 Actor 的数量，也可以说是在当前玩家一定范围内的 Actor 的数量。因为如果离得太远，由于游戏性能优化原因，是不会把非常远的 Actor 也获取的。

## 我们找Count来干什么？

找到 Count，我们也就找到了 UWorld、ULevel 和 ActorArray 的地址。因为 Count 的地址都是在 UWorld 里的 ULevel 下的。

ActorArray 在 UE4 里其实是一个 TArray 的结构体：
- 第一个变量为指向 Actor 指针数组的指针
- 第二个变量 Num 就是这个数组的长度，也就是 Count

所以 Count 的地址减去8字节，就是 ActorArray 的地址。获取这两个值后，就可以遍历所有 Actor。

本文以单机游戏 Battle Royale Trainer 为例进行分析。单机游戏相比网络游戏干扰较少，适合练习数据挖掘技巧。

## 搜索Count的动态地址

1. 首次扫描：4字节，值范围1-2000
2. 对靶子射击，搜索增加的数值（击中会创建伤害数值Actor）
3. 空枪测试，搜索未变动的数值
4. 重复以上步骤直到结果数量较少
5. 使用内存浏览器（Ctrl+B）查看结果，显示类型设为8字节
6. 寻找整齐的Actor数据（如图示0000020B）

![Count数据示例](/usr/uploads/2021/06/3564924053.png)

## 找到Count基址

1. 对Count地址进行指针扫描：
    - 设置最大2级偏移
    - 取消勾选"每个节点的偏移最大相差"
    - 注意扫描路径不能有中文

![指针扫描设置](/usr/uploads/2021/06/1205918573.png)

2. 观察扫描结果中不会闪动的行：
![指针扫描结果](/usr/uploads/2021/06/886573002.png)

3. 重启游戏验证地址有效性
4. 确认偏移值（UE4一般第一层偏移为30）

![最终结果](/usr/uploads/2021/06/2545629889.png)

## 最终基址数据

- UWorld: "BattleRoyaleTrainer-Win64-Shipping.exe"+02AEFFB8
- ULevel: UWorld -> 30
- Count: UWorld -> 30 -> B8
- ActorArray: UWorld -> 30 -> B0

## 联网游戏的特殊情况

联网游戏分析方法略有不同：
1. 受其他玩家干扰，需要更多次搜索来缩小范围
2. 可通过人物Z坐标等其他数据反推Count基址
3. 利用UE4引擎特性（ULevel偏移通常为0x30）辅助定位