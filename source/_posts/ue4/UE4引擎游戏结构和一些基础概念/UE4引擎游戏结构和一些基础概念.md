---
title: UE4引擎游戏结构和一些基础概念
date: 2021-06-21 02:16:00
categories: windows
urlname: 31
cover: /usr/uploads/2021/06/482392516.png
tags:
    - UE4
    - 游戏开发
    - 内存分析
---

## UE4游戏结构概览

小龙讲师总结的结构:
![UE4游戏结构图](/usr/uploads/2021/06/482392516.png)

## 常用结构概念

### UWorld
- 单例对象，逆向UE4游戏的重要入口
- 可获取UGameInstance和ULevel

### UGameInstance
UWorld类的属性:
```cpp
struct UGameInstance* OwningGameInstance;
```

### ULocalPlayer
UGameInstance类的属性:
```cpp
struct TArray<struct ULocalPlayer*> LocalPlayers;
```
数组的第一个元素即为当前角色的ULocalPlayer。

### APlayerController
ULocalPlayer父类(UPlayer)的属性:
```cpp
struct APlayerController* PlayerController;
```
包含APawn、APlayerCameraManager等重要数据。

### APawn
- 继承自AActor
- 在UE4逆向中称为InPawn
- 位于AController类:
```cpp
struct APawn* Pawn;
```

### ULevel
- 类似Unity中的Scene
- UWorld下的属性:
```cpp
struct ULevel* PersistentLevel;
```

### Actor
- 可放置在关卡中的Object基类
- 包含ActorComponents控制移动和渲染
- 支持网络属性复制
- 常见Actor: 玩家、敌人、物资、载具等

### Count
- 动态创建的Actor数量
- 游戏进行时递增
- 返回主界面时重置

### Actor数组
- 存放一定范围内的Actor
- 结合Count可遍历获取所有Actor
- 可获取Actor类型、坐标等数据

### GName
通过Actor ID获取蓝图类名，用于区分Actor种类。
