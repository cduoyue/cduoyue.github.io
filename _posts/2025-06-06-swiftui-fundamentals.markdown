---
layout: post
title:  "SwiftUI Fundamentals"
date:   2026-01-27 00:00:00 +0000
tags: [iOS, SwiftUI]
author: Neoren
excerpt: "A comprehensive guide to SwiftUI fundamentals for iOS development"
---
## History

- SwiftUI is a **UI framework**, introduced in **2019** on WWDC. 它采用**声明式**的方式，允许开发者通过**描述界面**的代码来构建UI

## Main Features

### 声明式界面构建

- 采用声明式语法，即只需**描述“界面是什么”**
- 开发者专注于声明UI的内容，底层细节由框架自动处理
- 所有UI元素都是遵循**View协议**的视图结构，且通过在视图上应用**视图修饰符**，可以修改视图的**样式、布局、响应交互**等属性

### 状态变化驱动视图更新

- 状态是**被观察**的，即SwiftUI能“订阅/追踪”的东西：@State (视图内部状态，适合简单值类型状态) / @Binding (父子视图间共享状态，实现单一数据源的双向绑定) / @StateObject (引用类型状态，视图内部创建并需要在视图周期内保持的对象) / @ObservedObject（配合 ObservableObject + @Published）(引用类型状态，由外部注入的可观察对象)/ @Environment & @EnvironmentObject (在更上层环境共享数据)/ @FocusState / @AppStorage / @SceneStorage，以及 Swift 5.9+ 的 @Observable（Observation）体系等（**学习的重中之重**）
- 普通数据（不在状态系统里）一般不会自动驱动更新

### 动画支持

- 提供简便的动画API
- 只需应用视图修饰符（如.animation())或内置函数（如withAnimation()），就能在**状态**变化时自动执行动画
- 动画是加在一次**视图更新**事务上的：如果某状态变化产生一次UI更新并被标记为带动画，那么只要更新里涉及到**可动画的属性变化**(position, opacity, scale, rotation, shape等)，就会过渡动画

### 视图组合与复用

- 鼓励将界面拆成小型的可重用视图组件（即**自定义视图结构**，通常对应一个View文件），然后用视图容器组合成复杂界面

### 自动布局适配

- 使用VStack等布局后，会根据不同的设备和屏幕尺寸自动调整布局

### 触摸和手势

- 框架内置了**事件处理机制**，来响应触摸、手势等各种输入

### 和UIKit的兼容性

- Integrate a SwiftUI view into a UIKit-based app (using a _**UIHostingController**_)

## 学习资源

- YouTube CS193p
- Stack Overflow

## SwiftUI Pathway

### 设计最合适的视图

- **Finding the right way** to display information across different views is crucial to keeping your app clean and functional
- A range of **container views**: 
  - Some purely for structure and layout (VStack, Grid)
  - Some also adopt system-standard visuals and interactivity (List, Form)
- **Choosing the most appropriate container views** is an important skill to learn

### Take a structured approach to design

- 做界面别靠“堆控件+调参数”碰运气，而是**先搭骨架、再逐层细化**
- 对应到SwiftUI，就是先把 View 的“结构”搭对（**容器与层级**），再用 **modifier** 做“表现”微调

### Plan for navigation

- Great navigation can go unnoticed; the easier your app is to interact with, the less you think about those interactions 

### Dive into data

- **Modeling data** is a complex part of any app

## SwiftUI essentials

### Basic protocols

- _**App**_: 
  - 唯一要求 @SceneBuilder var body: Self.Body { get } 其中Body: Scene
  - @main必须应用在遵循App协议的结构体上
- _**Scene**_: 
  - _**WindowGroup**_是一个遵循Scene协议的类型，其初始化器为 nonisolated init(@ViewBuilder makeContent: @escaping () -> Content) 其中Content: View，nonisolated意味着它没有被绑定在MainActor上，@escaping是因为闭包不是一次执行，而是被存到WindowGroup这个Scene值里，以后会多次调用。（WindowGroup定义的是一组可实例化的窗口场景，每次新建窗口时，都会通过同一个闭包重新生成视图内容，最终，你会在多个独立的窗口中看到相同的界面布局，但每个窗口是互相独立的视图实例）
  - Types of scenes: **WindowGroup, Window, DocumentGroup, Settings**
- _**View**_:
  - 核心要求 @ViewBuilder var body: Self.Body { get } 其中Body: View

### Important components

- **Buttons, state, and closures** are key components of SwiftUI. They enable your app to respond to user actions, which lets you create powerful and expressive interfaces

- Use **List, TextField, Bindings** to create dynamic content

  ```Swift
  @State private var items = ["Apple", "Banana"]
  @State private var newItem = ""
  
  List {
      ForEach(items, id: \.self) { item in
          Text(item)
      }
  }
  
  TextField("Add item", text: $newItem)
  Button("Add") {
      items.append(newItem)  // List 自动更新
  }
  ```

- _**@State**_: works only for value types, such as structures and enumerations

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260127232826238.png" alt="image-20260127232826238" style="zoom:50%;" />

- **Mechanism of Binding**:

  - Binding is a property wrapper

  - If we write _**@Binding var switchState: Bool**_, the Compiler will generate  **private var _switchState: Binding<Bool>**, **var switchState: Bool { get { _switchState.wrappedValue } nonmutating set { _switchState.wrappedValue = newValue } }**, and **var $switchState: Binding<Bool> { _switchState }**. The initilization process is:

    ```swift
    ChildView(switchState: $isOn)
    // 这里实际上是在初始化底层的_switchState，而不是表面的switchState
    
    struct Children: View {
      @Binding var switchState: Bool
      
      // 编译器生成的构造函数实际上是：
      init(switchState: Binding<Bool>) {
        self._switchState = switchState // 初始化底层存储
      }
    }
    ```

    <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260126122020258.png" alt="image-20260126122020258" style="zoom:50%;" />

    <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260126120109019.png" alt="image-20260126120109019" style="zoom:50%;" />

    State().wrappedValue 和 Binding().wrappedValue都指向同一个数据源！

- **Data modeling**: 

  - The art of representing real-world concepts as models
  - Includes **data & business logic**

- **Models and persistence** (_**SwiftData**_):

  - _**@Model**_ converts a Swift class into a stored model managed by **SwiftData**

  - The built-in **identity of the class** can make SwiftData uniquely identify and globally share the data object

  - _**ModelContainer**_是SwiftData的数据存储管理器

    ```swift
    let container = ModelContainer(for: Item.self)
    // 内存存储，用于测试：
    let container = try ModelContainer(
      for: Item.self, 
      configurations: ModelConfiguration(isStoredInMemoryOnly: true))
    ```

    可将其**注入环境**中：

    ```swift
    .modelContainer(container)
    // 或简化写法：
    .modelContainer(for: Item.self)

  - _**EnvironmentValues**_是用来承载“环境”的一组值的集合（一个结构体），通常不用直接创建，可通过两类API读写：

    ```swift
    // 读：@Environment
    @Environment(\.modelContext) private var modelContext
    @Environment(\.dismiss) private var dismiss
    @Environment(\.colorScheme) private var colorScheme
    
    // 写：环境修饰符
    someView()
    	.environment(\.colorScheme, .dark)
    
    .modelContainer(container)

  - _**@Query**_属性包装器：

    ```swift
    // 自动查询数据库并返回结果数组
    @Query private var items: [Item]
    
    // @Query内部类似这样的机制：
    @propertyWrapper
    struct Query<Element: PersistentModel> {
      @Environment(\.modelContext) private var context // 自动获取context
    	
      var wrappedValue: [Element] {
        return context.fetch(/*查询条件*/)
      }
    }
    ```

  - _**ModelContext**_: a connection between the view and the model container, so that you can fetch, insert, delete items in the container

- **Modal** interface:

  - To focus people on a **short, well-defined task** that they must complete or cancel before continuing
  - **Sheets** animate from the bottom of the screen, pushing the current view into the background
  - You can trigger a sheet to appear when an **optional property** has a value

## SwiftUI elements

### Basic views

```swift
Text("Hello, world!")
Spacer()
Divider()
Image(systemName: "globe"); Image(imageName)
Button("Roll") {}; Button("Remove Dice", systemImage: "minus.circle.fill") {}; Button {} label: {}; Button(action:) {}
ContentUnavailableView("no movies", systemImage: "film.stack"); ContentUnavailableView { Label("no friends", systemImage:"") }
// TextField(_ titleKey: LocalizedStringKey, text: Binding<String>)
TextField("Add Name", text: $nameToAdd)
	.autocorrectionDisabled()
	.onSubmit {
    Names.append(nameToAdd)
    nameToAdd = ""
  }
// Toggle(_ titleKey: LocalizedStringKey, isOn: Binding<Bool>)
// DatePicker(selection: Binding<Date>, label: () -> View)
ColorPicker("color", selection: $colors[0])
Picker("Favorite Movie", selection: $friend.favoriteMovie) {}
NavigationLink { MovieDetail(movie: movie) } label: { Text(movie.title) }
// NavigationLink(_ title: StringProtocol, destination: () -> View)
ToolbarItem(placement: .navigationBarTrailing) { EditButton() }
// ToolbarItem(content: () -> View)
Slider(value: $depth, in: 0...50) { Text("Depth") }
RoundedRectangle(cornerRadius: 17)
Tab("Friends", systemName: "person.and.person") { Text("Friends") }
// 一组动态子视图
ForEach(1…numberOfDice, id: \.description) { _ in DiceView() }
ForEach(names, id: \.description) { name in Text(name) }
ForEach(movies) { movie in } //Movie: Identifiable
ForEach($players) { $player in TextField("Name", text: $player.name) }
```

### Container views

```swift
VStack {}; VStack(alignment: .leading, spacing: 20) {} ; HStack {}; ZStack {}
ScrollView(.horizontal) {}
List {}
TabView {}
	.tabViewStyle(.page)
NavigationStack {}
NavigationSplitView {} detail: {}
Group {}
Section("Favorited by") {}
Grid {
  GridRow {}
  GridRow {}
}
```

### Modifiers

```swift
/* Nature of modifiers:
1) Most modifiers are methods of the extended View protocol
2) These modifiers typically return a new view that wraps the original view, using some View as return type
3) This design allows us to chain multiple modifiers
4) We can create our own modifiers
5) Other modifiers can be methods of specific view type */

.imageScale(.large)
.padding(); .padding(17); .padding(.horizontal, 17); .padding([.top, .leading], 17); .padding(EdgeInsets(top: 17, leading: 18, bottom: 16, trailing: 678))
.background(.purple, in: RoundedRectangle(cornerRadius: 17)); .background(Gradient(colors: gradientColors)); .background(.appBackground); .background(.bar); .background {}
.shadow(radius: 17); .shadow(color: .gray, radius: 17)
.foregroundStyle(.orange); .foregroundStyle(.tint); .foregroundStyle(.black, .white) // 主要前景、次要前景
.font(.headline); .font(.system(size: 78)); .font(.largeTitle.lowercaseSmallCaps())
.fontWeight(.semibold); .fontDesign(.monospaced); .fontWidth(.compressed)
.multilineTextAlignment(.center)
.border(.purple, width: 1.7)
.resizable()
.frame(width: 170, height: 170); .frame(maxWidth: 170, maxHeight: 170); .frame(maxHeight: .infinity); .frame(maxWidth: .infinity, alignment: .trailing)
.overlay(alignment: .topleading) {}
.rotationEffect(.degrees(180))
.tabViewStyle(.page)
.buttonStyle(.bordered)
.buttonBorderShape(.capsule)
.disabled(numberOfDice == 1)
.aspectRatio(1, contentMode: .fit)
.labelStyle(.iconOnly)
.tint(.white)
.symbolRenderingMode(.multicolor)
.autocorrectionDisabled()
.bold(); .bold(true)
.clipShape(RoundedRectangle(cornerRadius: 8))
// .focused(_ condition: FocusState<Bool>.Binding)
.focused($isTextFieldFocused)
.onSubmit {}
.onAppear {}
.contentShape(Rectangle())
.onTapGesture {}
.navigationTitle("Birthdays")
.navigationBarTitleDisplayMode(.inline)
.safeAreaInset(edge: .bottom) {}
.textFieldStyle(.roundedBorder)
.modelContainer(for: Item.self)
.gesture(
	DragGesture() // 遵循Gesture协议
  	.onChanged { value in
      // 变更状态，闭包被不停回调
    }
 )
.tabItem { Label("Movies", systemImage: "film.stack") }
// .onDelete(perform: Optional<(IndexSet) -> Void>)
.onDelete(perform: deleteItems)
.onMove { indices, newOffset in
  players.move(fromOffsets: indices, toOffset: newOffset)
}
// @ToolbarContentBuilder (result builder)
.toolbar {
  ToolbarItem(placement: ...) { ... }
  ToolbarItem(placement: ...) { ... }
}
.toolbar {
  Button() // 这里直接采用视图是应用了语法糖
}
// @State private var newMovie: Movie?  其中Movie遵循Identifiable
.sheet(item: $newMovie) { movie in NavigationStack {} }
.tag()
.searchable(text: $searchText)
.labelsHidden()
.stroke(lineWidth: 17); .strokeBorder(lineWidth: 17)
.offset(y: -170)
.gridColumnAlignment(.leading)
.controlSize(.large)
.accessibilityElement(children: .ignore)
.accessibilityLabel("time remaining")
.accessibilityValue("10 minutes")
```

