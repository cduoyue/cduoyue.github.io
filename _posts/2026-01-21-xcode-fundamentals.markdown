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

- Add images, strings (本地化字符串文件), data files and other resources to your projects, and manage how you load them at runtime
- Use _**Assets**_ to organize and manage resources such as images, colors, app icons.
- Use the image: (SwiftUI) _**let image = Image(“ImageName”)**_
- Use the accent color: (SwiftUI) _**.foregroundStyle(Color.accentColor)**_

### Localization

- 在代码中使用**国际化API**，然后加入**语言复数规则**的支持来提高翻译的精确性

- 先告诉 Xcode：我支持哪些语言；再告诉它：每种语言要用哪些资源文件

- 把项目里的本地化内容导出成文件，发给翻译人员。他们负责把界面上用户能看到的文字翻译成目标语言，并且根据不同文化和地区习惯调整相关资源。最后导入文件并在Xcode上测试

- 先**国际化**代码，使其“可翻译、可适配”；再**本地化**，真的把内容翻译成各语言/地区版本并接入项目

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260123182449671.png" alt="image-20260123182449671" style="zoom:50%;" />

- 正确国际化后，系统会根据语言/地区设置自动提取本地化资源

- **Foundation**及其它一些苹果框架，支持上述国际化

- 把你代码里“普通文字”改成通过 _**String(localized:)/AttributedString(localized:)**_ 从本地化资源里取，这样系统会按用户当前语言自动显示对应翻译

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260123183530935.png" alt="image-20260123183530935" style="zoom:50%;" />

  

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260123184146934.png" alt="image-20260123184146934" style="zoom:50%;" />

- SwiftUI 里那些接收 _**LocalizedStringKey**_ 的控件，直接写 "..." 会自动按本地化 key 处理

- 要用 **Foundation** 自带的**格式化器**来生成“可随语言/地区自动变化”的日期和数字字符串

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260123185910687.png" alt="image-20260123185910687" style="zoom:50%;" />

## Tuning and debugging 

### Debugging

- Identify and address issues in your app using the **Xcode debugger, Xcode Organizer, Metal debugger, and Instruments**

- 标准调试流程：**连调试器→复现→在关键位置下断点→停住后看变量/调用栈→一步步缩小直到找到根因**

- 找bug流程：

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260123195622796.png" alt="image-20260123195622796" style="zoom:50%;" />

- 当 App 崩溃、抛异常、或出现运行时问题时，你想直接看 crash 的 **stack trace（调用栈）**去定位问题，往往会很难，因为：

  - 调用栈里显示的那一行，不一定是“根因那一行”
  - 它常常只是最后出事的地方（症状点），而真正把状态弄坏/把非法数据传进来的代码，可能在更早之前、在别的线程、或更上游的函数里

- 当某bug难下断点时，使用_**symbolic breakpoint**_ 或_**issue breakpoint**_，它们能快速定位，因为它会在“问题刚发生的那一刻”停住，你立刻能看到：（不确定断点该下哪，就让断点去抓“异常/特定函数调用”这个现象点，然后用调用栈反推到你的代码根因）

  - 调用栈（谁触发的）
  - 线程（是否并发相关）
  - 变量现场（参数是否不合法）

- Xcode调试器按钮：

  - _**Step Over**_: 用于执行当前行的代码，但不会进入当前行中调用的函数或方法内部
  - _**Step In**_: 让调试器进入当前行代码中调用的函数或方法内部，从而逐行调试该函数的执行过程
  - _**Step Out**_: 作用是退出当前函数的调试，继续执行直到当前函数返回到调用它的位置，然后在返回处暂停

### Performance and metrics

- Cycle of continuous improvements:

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260123195515927.png" alt="image-20260123195515927" style="zoom:50%;" />

- **iOS watchdog timer**（看门狗定时器）: iOS系统中一个监控应用响应性的机制。它主要在应用启动、挂起、恢复等关键时刻监视主线程的执行情况，如果应用在规定时间内未能及时响应或发生阻塞，系统会认为应用失去响应能力，从而强制终止该应用。这样做可以防止长时间无响应导致系统卡顿或崩溃，保证用户体验和系统稳定性。

- To thoroughly understand your app’s **performance**, combine information from multiple sources:

  - _**Xcode Organizer**_: 性能数据的主要来源是，用户设备在获得用户同意（共享分析/诊断）后，由系统采集的性能/诊断指标（MetricKit 等），再通过 Apple 服务器聚合后提供给开发者在 Organizer 里查看。(你的 App 需要通过 TestFlight 或 App Store 分发到真实设备使用，数据才会逐步累积并在 Organizer 出现)
  - _**MetricKit**_: Apple 提供的框架，用来让你的 App 接收系统在设备上采集的性能指标和诊断信息。接收有两种途径，一是在Organizer查看；二是在**代码里订阅MetricKit**
  - _**TestFlight**_: 测试者的**反馈信息**
  - _**App Store**_: 所有使用者的**反馈信息**

- 用 _**Instruments**_ 给你的 App 做性能剖析（profile），并且选择一个“和你要关注的指标匹配”的模板（profiling template）

  - Hangs: _**Time Profiler**_ template
  - Memory: _**Allocations**_ and _**Leaks**_ templates
  - Energy: _**Energy Log**_ template

  You get **higher-fidelity** measurements by profiling on a **device** instead of the **simulator**

- You may need to make **significant architectural changes** to your app to improve the performance 

### Testing

- Develop and run tests to detect **logic failures, UI problems, and performance regressions**

- **Test Pyramid**:

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260123232327121.png" alt="image-20260123232327121" style="zoom: 50%;" />

- _**Performance test**_: 为了防止性能退化，开发者通常会在持续集成过程中引入性能测试和基准测试来**监控关键代码区域**，及时发现并修复潜在的性能问题

- _**Swift Testing**_：用于单元测试和集成测试

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260124001312755.png" alt="image-20260124001312755" style="zoom:50%;" />

- _**XCTest**_：用于单元测试、集成测试、UI测试、及性能测试

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260124001436273.png" alt="image-20260124001436273" style="zoom:50%;" />

- **AAA (Arrange–Act–Assert)**结构：把一个测试函数按**“准备→执行→验证”**分成三段，便于读、写、维护。

  - Arrange (准备): 搭建测试环境与输入，例如创建被测对象（SUT）、准备 mock/stub、设置初始数据、构造参数（Arrange 就是把舞台搭好，把不稳定/昂贵的依赖换成可控的“假实现”，让测试又快又稳定）
  - Act (执行)：做一次你要测试的动作，例如调用某个方法、触发一次事件、执行一个异步任务
  - Assert (断言)：检查结果是否符合预期，例如XCTAssertEqual、XCTAssertTrue，验证返回值、状态变化、调用次数等

- Integration test: looks very similar to unit test, use the same APIs, follow the same AAA pattern. It examines the behaviour of a larger subsystem, or combination of classes and functions

- 在集成测试的Arrange阶段，应该让**更多“真实项目代码”**参与测试，因此**少用一些存根**。因为集成测试的目的就是验证“多个模块拼在一起能不能正常协作“，所以要扩大被测范围，让真实的组件一起跑

- UI 测试用 XCTest 写，但它通过**操作界面**来测用户流程，而不是直接调用代码

- **Performance test**: XCTest runs your code multiple times, measuring the requested metrics. You can set a baseline expectation for the metric, and if the measured value is significantly worse than the baseline, XCTest reports a test failure

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260124125330923.png" alt="image-20260124125330923" style="zoom:50%;" />

- **Test Plan**：更细粒度地管理“测试跑哪些、怎么跑、用什么配置”。在_**Product -> Test Plan**_中

- **Tags（标签）**是给测试用例加的“语义分类信息”

- 如果你把某些 tag 加在一个 test suite（测试套件/测试分组，比如一个测试类型/测试集合） 上，那么这个 suite 里的所有测试都会自动继承这些标签——不用每个测试函数再重复写一遍

## Distribution and continuous integration 

### Distribution

- Prepare your app and share it with your team, beta testers, and customers

- Provide all the required information about your app (choose the settings carefully)

- 版本管理：主版本号(Major).次版本号(Minor).维护版本号(Patch)

- To distribute, first create an _**Archive**_ of your app. Archive = 一次“可分发版本”的构建快照，Xcode 会把它保存成一个 .xcarchive bundle（一个目录包）。它包含：

  - .app (可执行文件 + 资源)
  - 符号/调试信息（最典型是 dSYM，用于崩溃符号化、性能分析等）
  - 一些元数据（Info、签名相关信息、BCSymbolMaps 等，视构建设置而定）

  所以它不是单纯的“Release build”，而是 Xcode 为“后续分发/符号化/上传”准备的完整产物集合。Archive 本身只是**“原材料包”**。当你在 Organizer 里选择 Distribute App 并选不同的分发方式时，Xcode 会把 archive 里的内容按目标分发场景重新导出/重新打包，例如：

  - 导出为 _**IPA**_（或上传到 App Store Connect/TestFlight）
  - 按你的选择做**重新签名**
  - 可能做 **App Thinning**（按设备切片）、处理符号文件随不随包走等
  - 按导出选项生成对应的结构与文件

  Archive 是“包含符号信息的构建存档”；Distribution 是“从存档里按指定渠道规则导出成最终可安装/可上传的包”

### Xcode Cloud

- Automatically build, test, and distribute your apps with Xcode Cloud to verify changes and create high-quality apps

- Xcode Cloud lets you adopt **C**ontinuous **I**ntegration/**D**elivery (CI/CD)…It combines Xcode, TestFlight, and App Store Connect

- 持续集成/交付的传统做法是自己搭建服务器或使用第三方服务。Xcode Cloud则是苹果官方提供的云端解决方案，免去了自行搭建的麻烦

- Xcode Cloud主要通过以下方式将Xcode与**云技术**相结合：

  - 云端构建：Xcode Cloud会在苹果提供的**云服务器**上拉取你的代码仓库（GitHub等），自动执行编译与打包，减少对本地环境的依赖
  - 自动化测试：你可以配置各种测试流程（单元测试、UI测试等），Xcode Cloud会在**云端模拟器或设备**上运行测试并生成报告，无需本地手动执行
  - 分发管理：构建完成后，Xcode Cloud可以直接将测试包分发到TestFlight，也可为生产环境做准备
  - Xcode内部集成：你可以在Xcode的项目设置中直接配置和查看_**Xcode Cloud工作流**_，监控构建/测试状态

- CI/CD开发循环图：

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260124154451238.png" alt="image-20260124154451238" style="zoom:50%;" />

- 



