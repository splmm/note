---
title: 利用github Action编译安卓内核驱动
layout: post
date: 2024-11-10 04:58:06
categories: 安卓
tags: 
    - 内核编译
    - Android
    - GitHub Actions
    - GKI
description: 本文介绍如何利用GitHub Actions来编译安卓内核驱动，特别适用于本地内存不足的情况。文章详细讲解了GKI内核的编译流程和自动化构建过程。
---
## 背景
由于需要利用安卓内核驱动进行游戏数据读取，但编译安卓内核需要电脑内存32G以上，而本机只有8G，所以选择利用GitHub Actions进行云端编译。

## 内核类型说明
安卓内核分为两种类型：
- GKI内核：谷歌推荐的标准化内核
- 非GKI内核：传统的碎片化内核

> 通过查看"关于本机"可以看到内核版本，5.0以上版本为GKI内核，本文主要讲解GKI内核的编译方法。

## 编译准备
1. 确认内核版本
   ![手机内核版本](myphone.jpg)
   - 例如：5.10.149 对应android12内核版本
   > 注意：此处的安卓版本指内核版本，不是系统版本

2. 确认内核分支
   - 参考[谷歌官方文档](https://source.android.google.cn/docs/setup/reference/bazel-support?hl=zh-cn)
   ![内核分支](image.png)
   - 本例使用：common-android12-5.10

3. 查看构建方法
   ![构建文档](image-1.png)
   ![构建命令](image-2.png)

## GitHub Actions配置
创建workflow文件，配置自动化构建流程：

```yaml
# 工作流名称为 Android12-5.10
name: Android12-5.10

# 触发条件配置
on:
    workflow_dispatch:  # 允许手动触发工作流执行

# 定义工作流中的作业
jobs:
    build:
        # 指定运行环境为最新版Ubuntu
        runs-on: ubuntu-latest
        
        steps:
            # 步骤1: 检出代码仓库
            - name: Checkout repository
                uses: actions/checkout@v3
                
            # 步骤2: 修改内核版本代码定义
            - name: Modify MY_LINUX_VERSION_CODE
                run: |
                    sed -i '/#ifndef MY_LINUX_VERSION_CODE/a #define MY_LINUX_VERSION_CODE KERNEL_VERSION(5,10,43)' kerneldriver/ver_control.h

            # 步骤3: 安装repo工具
            - name: Install repo tool
                run: |
                    curl https://storage.googleapis.com/git-repo-downloads/repo > /usr/local/bin/repo
                    chmod a+x /usr/local/bin/repo 

            # 步骤4: 设置Android内核源码
            - name: Set up Android Kernel source
                run: |
                    mkdir -p android-kernel && cd android-kernel
                    repo init -u https://android.googlesource.com/kernel/manifest -b common-android12-5.10 --no-repo-verify --depth=1
                    repo sync --force-sync --no-clone-bundle --no-tags -f -c -j32

            # 步骤5: 复制内核驱动文件
            - name: Copy kerneldriver
                run: |
                    cd android-kernel
                    mkdir -p common/drivers 
                    cp -r ../kerneldriver common/drivers 

            # 步骤6: 修改Makefile添加驱动编译配置
            - name: Modify Makefile
                run: |
                    cd android-kernel
                    echo "obj-y += kerneldriver/" >> common/drivers/Makefile

            # 步骤7: 构建Android内核
            - name: Build Android Kernel
                continue-on-error: true
                run: |
                    cd android-kernel
                    BUILD_CONFIG=common/build.config.gki.aarch64 LTO=thin build/build.sh -j32

            # 步骤8: 上传编译好的驱动文件
            - name: Upload kerneldriver.ko
                uses: actions/upload-artifact@v3
                with:
                    name: kerneldriver.ko
                    path: android-kernel/out/android12-5.10/common/drivers/kerneldriver/kerneldriver.ko 
```

## 构建结果
成功编译后的文件：
![构建结果](image-3.png)
![驱动文件](image-4.png)
![完成](image-5.png)

## 使用方法
1. 下载编译好的kerneldriver.ko
2. 上传到手机
3. 使用insmod命令加载驱动
>我自己的内核驱动就不放出来了，自己慢慢稳定，嘿嘿

