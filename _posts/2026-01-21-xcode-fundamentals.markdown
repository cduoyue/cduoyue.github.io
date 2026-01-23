---
layout: post
title:  "Xcode Fundamentals"
date:   2026-01-21 
tags: [iOS, Swift]
author: Neoren
---
## History

- **2003**, original Xcode (**IDE**, developed by Apple)
- 2008, Xcode 3.1, integrated the **iOS SDK**
- 2014, Xcode 6.0, introduced the **Swift** language 

## Main Features

### Summary

- Xcode is the **suite of tools** you use to build apps for Apple platforms
- Use Xcode to manage your **entire development workflow**

### Code editor

- 语法高亮、智能代码完成
- 快速导航、代码折叠
- 代码重构

### Simulator

- 模拟器可以在 Mac 上模拟各种 Apple 设备的运行环境，让你快速验证应用的外观和功能
- 可选择不同型号设备、不同版本OS，测试兼容性
- 丰富的**调试支持**，模拟定位、内存警告、网络状况等场景，帮开发者在各种环境下测试应用

### _Debugger_

- _**LLDB**_ (LLVM Debugger): 运行时设置断点暂停代码执行，逐步检查代码逻辑；允许查看当前调用栈、监视变量变化、检查对象内存
- _**Instruments**_: 性能分析工具，收集CPU、内存、GPU等指标数据，帮助找出性能瓶颈和内存泄漏等问题

### Testing

- Xcode集成了测试**框架**和**工具**，方便开发者编写和执行测试用例
- Xcode支持Unit Testing和UI Testing
- Xcode 11引入Swift Testing框架
- 对UI测试，XCTest提供了XCUITest接口，能自动控制应用的 UI 进行点击、输入等操作，从而模拟用户行为来测试界面流程

### Git

- Xcode会自动为项目初始化一个Git仓库，引入Git系统

### Other tools

- Icon Composer
- Reality Composer
- Create ML
- Accessibility Inspector
- FileMerge

## Essentials

### Building

- Compile: 将源代码转化为中间目标文件
- Link: 将各个目标文件和库文件整合生成最终的可执行文件或框架
- Package/Code Sign: 将编译好的二进制文件、资源文件、配置文件等整理到应用包中，并进行代码签名等后续处理

### Target

- Target就是项目中一个**独立的构建单元**，它告诉Xcode如何从源代码和资源生成最终的产品
- Target本质上是一套构建指令，它告诉Xcode：要构建什么、如何构建、构建的配置
- Target包括：源代码文件、资源文件、依赖关系、构建设置、Info.plist、签名和证书

## Xcode IDE

### Symbol

- Xcode世界里，Symbol指的是编译器为每个命名的代码元素生成的唯一标识符
- 在Swift编译后，每个符号(symbol names)都会被转化成一个唯一的链接符号(symbol)，存在目标文件和符号表中

### Pattern token

- 模式标记，是预定义的通配片段，用来在“全局搜索”（Find navigator）里匹配一类字符，而不是具体文本
- 典型的token：空白符、URL、16进制数
- 可以和普通文字混用

### Info.plist

- Information property list, 它是iOS/macOS应用程序的核心配置文件
- 定义了应用程序的元数据和配置信息

### Remote-tracking branch

- origin/main
- 远程跟踪分支 = 你本地仓库里，专门用来记录“某个远程分支上次被你同步到本地时长什么样“的只读指针（或称快照）
- Xcode中点Fetch，远程跟踪分支更新到远程新提交；点Pull，把origin/main的变化合并/变基到你的本地main

### Restore a previous commit

- First, make sure you don’t have any uncommitted changes. If you do, either discard the changes (_**Integrate -> Discard All Changes…**_) or save them to apply later by stashing them (_**Integrate -> Stash Changes…**_)
- Open the _**Source Control navigator**_
- In the _**Repositories**_, expand your repository and the _**Branches**_ folder
- Select the branch that contains the commit you want to restore
- In the detail view, _**Control-click**_ the desired commit and choose _**Switch to “[commit-hash]”**_
- In the confirmation dialog, click _**Switch**_. Restore success
- 在该Commit上新建分支，提交修改

### Merge code changes

- Open the _**Source Control navigator**_
- In the _**Repositories**_, expand your repository and the _**Branches**_ folder
- _**Select the branch you want to merge into**_, and then _**Control-click**_ and choose _**Switch**_ to make it the current branch
- _**Select the branch you want to merge from**_, and then _**Control-click**_ and choose _**Merge [from branch] into [to branch]**_
- If there are no conflicts, Xcode complete the merge

### Capabilities

- Enable services that Apple provides, such as In-App Purchase, Push Notifications, Apple Pay, iCloud and many others

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260122221051873.png" alt="image-20260122221051873" style="zoom:50%;" />

### Build system

- Compile your code into a **binary** format, and customize your **project settings** to build your code
- When you tell Xcode to build your project, the build system analyzes your files and uses your project settings to assemble the **set of tasks** to perform 
- Xcode has **hundreds of build settings** to support the tools and steps associated with the build process
- To restore a setting’s original value, _**select it**_ and press the _**Delete**_ key

## Code

### Source Editor

- To open a new window tab, press _**command  T**_
- To show the library, press _**shift command L**_
- Annotate your code with _**// MARK: -**_, _**// TODO:**_, _**// FIXME:**_

### Swift packages

- Create reusable code, organize it in a lightweight way, and share it across Xcode projects and with other developers
- Swift packages bundle **source files**, **binaries** (预编译第三方框架/库，通常 xcframework), and **resources**
- package 可以是**库/命令行工具/插件**，但 iOS App 通常不是 package
- Xcode supports creating and publishing Swift packages
- Only add package dependencies by **trustworthy authors**
- Selecting the _**version**_ requirement is the **recommended** way to add a package dependency

## Interface

### Asset management 

