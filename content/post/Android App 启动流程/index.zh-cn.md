---
date: 2025-10-23T15:42:32+08:00
title: Android App 启动流程
description: 详细记录下Android App 启动流程
readingTime: true
image: 'the Android App startup process.png'
categories:
    - Android
tags:
    - Android
toc: true
---

# Android App启动完整流程详解


## 1. 系统层启动流程

### Launcher点击应用图标

- `Launcher`进程捕获用户点击事件
- 通过`PackageManagerService`查询目标应用的`AndroidManifest.xml`信息
- 获取应用的启动`Intent`和组件信息
- 调用`ActivityManagerService`(AMS)发起启动Activity的请求

### AMS处理启动请求

- `ActivityManagerService`接收启动请求
- 检查应用权限、用户权限和应用状态
- 通过`ProcessManager`判断是否需要创建新进程
- 如果需要新进程，则通过`Zygote`进程孵化新的应用进程
- `Zygote`通过fork机制创建新的应用进程，并初始化基本运行环境

## 2. 应用进程初始化

### Application启动

- 创建`Application`对象实例
- 调用`Application.onCreate()`方法
- 在此阶段可以初始化全局配置、第三方SDK、数据库等
- 执行`ContentProvider`的初始化（如果有声明）

### 主线程准备

- 创建主线程的`Looper`对象
- 初始化`Handler`消息机制
- 准备主线程的消息循环队列
- 确保UI线程可以处理各种系统事件和用户交互

## 3. Activity启动流程

### Activity创建阶段

- 通过`Instrumentation`创建`Activity`实例
- 调用`Activity.onCreate()`方法
- 执行`setContentView()`加载布局文件
- 初始化UI组件和数据绑定
- 调用相关Fragment的创建和添加逻辑

### 生命周期回调

- `Activity.onStart()`: Activity变为可见状态
- `Activity.onResume()`: Activity获得焦点，可以接收用户交互
- 此时Activity进入运行状态，显示在前台

## 4. 界面显示阶段

### 视图渲染过程

- 构建`View`树结构
- 执行`View`的测量(measure)、布局(layout)、绘制(draw)三大流程
- 通过`SurfaceFlinger`服务进行界面合成
- 利用GPU加速渲染界面内容

### 用户交互就绪

- 注册触摸事件监听器
- 建立完整的事件分发机制
- 应用进入正常运行状态，可以响应各种用户操作
- 系统开始处理各种输入事件和后台任务