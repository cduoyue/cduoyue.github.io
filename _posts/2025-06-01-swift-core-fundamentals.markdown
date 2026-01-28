---
layout: post
title:  "Swift Fundamentals"
date:   2026-01-21 
tags: [iOS, Swift]
author: Neoren
---
## History

- Swift was born in WWDC 2014 (by Apple’s Developer Tools Department) & got open source license in 2015.
- Almost all frameworks are closed.

## Main Features

- A strong type system and safety-oriented design:
  - Statically typed language (safe)
  - Optional (core of safety design, eliminate dangling pointers and null pointers)
  - Modern error treating

- Modern syntax & advanced features:
  - (syntax) concise & expressive
  - A major highlight of Swift’s **functional programming** features is the extensive use of closures. Function is a first-class citizen, & can be passed as a parameter or as a return value
  - Support **generic programming**
  - Emphasize **protocol-oriented programming**, allowing you to provide default implementations for protocols through protocol extensions.
- ARC (automatic reference counting)  
  - Strong reference cycle: weak, unowned, [ weak self ]
- Concurrency support
  - async/await, Actor

- Value type (struct/enum) & reference type (class)
  - Compared with structs, classes support features that structs don’t have, such as inheritance, type casting, deinitializers, and reference counting. These features make classes better suited to scenarios that require shared mutable state and follow an object-oriented model… Structs, because they are copied on assignment, are better suited for representing immutable or value-semantic data… Apple’s official recommendation is to use structs by default to encapsulate simple data, and only use classes when you need the class-specific features mentioned above

- Escaping closure
  - Strong reference cycle, capture list [ weak/unowned self ]
  - Can’t capture inout parameters

- Property wrapper

  - Construction:

    ![image-20260114101428895](/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260114101428895.png)

    Compiler will generate:

    ![image-20260114101453631](/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260114101453631.png)

  - Property wrapper can be applied to the “stored property of the type”, “local variable”, “functional parameter”; but can’t be applied to the “computed property” or “property with customized get/set”, and not in protocol definition

- Result builder

  - Embeded DSL (domain-specific language)
  - Use declarative-style code blocks to construct a final result according to custom composition rules
  - Working principle: A type marked with @resultBuilder provides a set of static methods (such as buildBlock, etc.). When the compiler encounters a closure annotated with the corresponding attribute, it doesn’t execute the block in the normal order; instead, it transforms the block into calls to those static methods

- Structure your code
  - Understanding **Building blocks** available to you when structuring your code and modeling your data
  - Write **organized codebase**

## Core syntax

### Basic types

- String & Character: 
  - 21-bit **unicode scalar** (code point 减去 代理区)
  - UTF-8/16/32 (**U**nicode **T**ransformation **F**ormat)
  - SSO (**S**mall **S**tring **O**ptimization)小字符串优化（短字符串）、缓冲区存储（长字符串）
  - String.Index (instance properties: startIndex, endIndex; instance methods: index(before:), index(after:), index(_:offsetBy:))
  - insert(_:at:) & insert(contentsOf:at:)
  - remove(at:) & removeSubrange(_:)
  - string1 == string2 (canonically equivalent 规范等价，即文本意义相同)
  - String instance properties: utf8, utf16, unicodeScalars, of type String.UTF8View, String.UTF16View, unicodeScalarView.

- Collection types (Array, Set, Dictionary):
  - Array syntax sugar: Array<Int>(arrayLiteral: 1, 2, 3) == Array<Int>([1, 2, 3]) == [1, 2, 3]
  - array.enumerated()返回一个懒序列EnumeratedSequence
  - Set must store **hashable** type, set.sorted()对Set<String>返回一个已排序的[String]
  - Dictionary’s subscript returns an optional
- Int, Double, Bool, Optional

### Control flow

- for-in (Sequence protocol: makeIterator(); IteratorProtocol: next())
- if-else, switch-case, guard-let-else
- while, repeat-while

### Function and closure 

- Function: reference type, parameters - constant by default 
- Closure: light weighted syntax, capture values from surrounding, escape closure, variable boxing, reference type

### Enumeration

- raw value (Planet.earth.rawValue), associated values

- CaseIterable, allCases

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260117140522808.png" alt="image-20260117140522808"  />

- Capabilities of properties (computed property & static stored property), methods, initialization, extension, and protocols

### Class and structure 

- Classes have additional capabilities: inheritance, type casting, deinitilizers, reference counting
- Lazy properties: lazy var
- Property observer: willSet, didSet. Added in 1) stored properties that you define; 2) stored/computed properties that you inherit (利用override增加钩子)
- Property wrapper: **syntactic sugar** for a property with a getter and a setter
- By default, the properties of a value type can’t be modified from within its instance methods.
- Initilization: default initializer, memberwise initializer, initializer delegation, designated initializer & convenience initializer, class initialization is a two-phase process, failable initializer

### Error handling

- Error protocol (empty): to identify a type that can be used as an error type
- Four ways to handle: propagation, do-catch, try?, try!

### Concurrency

- Swift has built-in support for writing asynchronous and parallel code in a **structured** (异步任务的创建、生命周期、取消、错误传播，都被约束在清晰的父子层级里) way
- Data race: multiple pieces of code try to access some piece of shared mutable state (use language-level support for concurrency, Swift detects and prevents data races)
- 要 await，先让自己变 async；如果不能变，就在 Task {} 里等
- for-await-in loop: AsyncSequence protocol
- async-let: –> Task { await … }
- Instance of TaskGroup: explicitly add child tasks, more control over priority and cancellation, dynamic number of tasks
- Tasks in a hierarchy (structured concurrency): each task in a given task group has same parent task, and each task can have child tasks
- Advantages of the explicit parent-child relationship between tasks: 
  - In a parent task, you can’t forget to wait for its child tasks to complete (父任务在离开作用域前，一定要等待所有子任务完成)
  - When setting a higher priority on a child task, the parent task’s priority is automatically escalated
  - When a parent task is canceled, each of its child tasks is also automatically canceled
  - Task-local values (任务本地值，“跟着当前任务走的上下文存储”) propagate to child tasks efficiently and automatically
- Task cancellation: cooperative cancellation model
- Responding to task cancellation:
  - Throwing an error like CancellationError
  - Returning nil or an empty collection
  - Returning the partially completed work
- group.addTaskUnlessCancelled: if cancelled, return “false” & Task not added in group
- Task.withTaskCancellationHandler(operation:onCancel:isolation:) 的onCancel闭包中可写入即使取消通知(钩子), 但任务的取消仍然是协作式的
- Unstructured task: don’t have a parent task, complete flexibility but also completely responsible for their correctness, call Task.init(name:priority:operation:) initializer (new task defaults to running with the same actor isolation, priority, and task-local state as the current task), call Task.detached(name:priority:operation:) static method (new task defaults to running without any actor isolation and doesn’t inherit the current task’s priority or task-local state)
- Data isolation:
  - Immutable data
  - Data referenced by only the current task (local variable, closure capture error)
  - Data protected by an actor
- actor: an object that protects access to mutable data by forcing code to take turns accessing that data
- main actor: 
  - actor MainActor { static shared: MainActor = MainActor() } MainActor.shared是个全局单例actor, 初始化器可能是private
  - Protects all of the data that’s used to show the UI
  - Takes turns rendering the UI, handling UI events, and running code that you write that needs to query or update the UI (串行化UI访问)
  - @MainActor:
    - @MainActor function的意思是：这段函数被标记为 “隔离到 MainActor 这个全局 actor 上执行”
    - To ensure that a closure runs on the main actor, you write @MainActor before the capture list and before in, at the start of the closure
    - You can also write @MainActor on a structure, class, or enumeration to ensure all of its methods and all access to its properties run on the main actor
    - When you’re building on top of a framework, that framework’s protocols and base classes are typically already marked @MainActor, so you don’t usually write @MainActor on your own types in that case
  - MainActor.run {}
  - Before you start using concurrency in your code, everything runs on the main actor
  - Closely related to the main thread, but they’re not the same thing. The main actor has private mutable state, and the main thread serializes access to that state. When you run code on the main actor, Swift executes that code on the main thread.
- actor.property的访问:
  - Isolated local state
  - Can be accessed by code running as part of the actor
  - 对于外部代码，需要向actor发出请求，进入串行执行队列，使用await
  - Called the “**actor isolation**”
- Concurrency domain: 隔离的并发小世界，里面有一组可能会被读写的可变状态
- **sendable** type: a type that can be shared from one concurrency domain to another
- Sendable protocol: empty, semantic requirements (让编译器验证你声明的类型是不是真的能安全跨并发域传递)
- Three ways to be Sendable:
  - Value type, and its mutable state is made up of other sendable data
  - The type doesn’t have any mutable state, and its immutable state is made up of other sendable data
  - The type has code that ensures the safety of its mutable state, like a class that’s marked @MainActor or a class that serializes access to its properties on a particular thread or queue

### Macros

- Two kinds of macros: 

  - Freestanding macro: appear on their own
  - Attached macro: modify the declaration that they are attached to

- Freestanding macro:

  - **#function**: calls the **function()** macro from the Swift standard library
  - **#warning**: calls the **warning(_:)** macro from the Swift standard library to produce a custom compile-time warning

- Attached macro: @OptionSet<Int>

- Macro’s declaration: name, parameters, where it can be used, what kind of code it generates (API instruction)

- Macro’s implementation (是个编译期插件): contains the code that expands the macro by generating Swift code

- public **macro** OptionSet<RawType>() = #externalMacro(module: "SwiftMacros", type: "OptionSetMacro") (Part of the declaration, using a macro to tell Swift where the implementation is located)

- Roles: @attached(member), @attached(extension, conformance: OptionSet), @freestanding(expression), @attached(member, names: named(RawValue), named(rawValue), named(‘init’), arbitrary)

- 抽象语法树AST：因为源码本质上是有层级结构的东西，用“树”最自然也最省事地把这种结构表示出来，编译器后续工作才能可靠进行。（源码是“句子”，树是“语法分析后的句法结构图”）

- let magicNumber = #fourCharacterCode("ABCD") –> AST

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260120210420436.png" alt="image-20260120210420436" style="zoom:50%;" />

- Swift helps macro authors avoid accidentally reading other input by restricting the code that implements macros:

  - The AST passed to a macro implementation contains only the AST elements that represent the macro, not any of the code that comes before or after it
  - The macro implementation runs in a sandboxed environment that prevents it from accessing the file system or the network

- The macro’s author is responsible for not reading or modifying anything outside of the macro’s inputs. For example, a macro’s expansion must not depend on the current time of day.

- Swift expands macros in the following way:

  - The compiler reads the code, creating an in-memory presentation of the syntax (AST, abstract syntax tree)
  - The compiler sends part of the AST (the macro call) to the macro implementation 
  - The compiler replaces the macro call with its expanded form
  - The compiler continues with compilation, using the **expanded source code**

- To implement a macro, you make two components: A type that performs the macro expansion, and a library that declares the macro to expose it as API. These parts are **built separately** from code that uses the macro, even if you’re developing the macro and its clients together, because the macro implementation runs as part of building the macro’s clients (宏实现是构建宏客户端的编译器插件，需先在另一个target里构建)

- To create a new macro using **Swift Package Manager**: run **_swift package init --type macro_** (creates several files, including a template for a macro implementation and declaration)

- To add macros to an existing project (a package):

  - Edit the beginning of your **Package.swift** file as follows:

    - Set a Swift tools version of 5.9 or later in the swift-tools-version comment (**_// swift-tools-version: 5.9_**)
    - Import the **CompilerPluginSupport** module
    - Include **macOS 10.15** as a minimum deployment target in the **platforms** list

    <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260120230300233.png" alt="image-20260120230300233" style="zoom:50%;" />

  - Add a **target** for the macro implementation and a **target** for the macro library to your existing **Package.swift** file

    <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260120231502230.png" alt="image-20260120231502230" style="zoom:50%;" />

  - Add a **dependency** on **SwiftSyntax** in your Package.swift file

    <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260120232011147.png" alt="image-20260120232011147" style="zoom:50%;" />

  - **Wait…**

### Type casting

- as?, as!
- Two special types: Any, AnyObject
- is, as

### Extensions

- Extensions can:
  - Add computed properties
  - Define methods
  - Provide convenience initializers
  - Define subscripts
  - Define and use new nested types
  - Conform to a protocol 
- Extension **can’t**:
  - Override existing functionality
  - Add new designated initializers or deinitializers to a class
  - Add property observers to existing properties

### Protocol

- A protocol defines a **blueprint** of methods, properties, and other requirements that suit a particular task or piece of functionality. 
- In addition to specifying requirements that conforming types must implement, you can **extend** a protocol to **implement** some of these requirements or to implement additional functionality that conforming types can take advantage of
- Property requirements are always declared as **variable** properties
- Default values can’t be specified for method parameters within a protocol’s definition
- Initializer requirement for class marked with **required**
- A **failable** initializer requirement can be satisfied by a failable or nonfailable initializer on a conforming type. A **nonfailable** initializer requirement can be satisfied by a nonfailable initializer or an implicitly unwrapped failable initializer
- Empty protocols (Sendable, Copyable, BitwiseCopyable etc.) to describe **semantic requirements** (about how values of those types behave and about operations that they support)
- Code with an **opaque type** works with some type that conforms to the protocol
- Code with a **boxed protocol type** works with any type, chosen at runtime, that conforms to the protocol…Because of this flexibility, Swift doesn’t know the underlying type at compile time, which means you can access only the members that are required by the protocol
- Delegation – protocol (encapsulate handed off responsibilities)
- Swift can automatically provide the protocol conformance for **Equatable**, **Hashable**, and **Comparable** in many simple cases
- A protocol can inherit one or more other protocols
- You can limit protocol adoption to class types by adding the **AnyObject** protocol to a protocol’s inheritance list
- Protocols can be **extended** to provide **method, initializer, subscript, and computed property implementations** to conforming types
- When you define a protocol extension, you can specify **constraints** that conforming types must satisfy before the methods and properties of the extension are available

### Generics

- Functions and types
- Protocol: associatedType
- Type constraints: T must inherit from a specific class, or conform to a particular protocol 

### ARC

- 类的引用：本质上就是一个指向堆上实例的指针，但不是裸指针，而是一个由编译器管理、带类型信息的安全引用
- Strong/weak reference: strong reference cycle, weak & unowned
- Closure retain cycle: **closure capture list**, unowned & weak

### Memory safety

- Multithreaded code: **Thread Sanitizer** (detect conflicting access, _**Xcode -> Product -> Scheme -> Edit Scheme -> Diagnostics -> 勾选Thread Sanitizer**_)
- A conflict occurs if:
  - The accesses aren’t both reads, and aren’t both atomic
  - They access the same location in memory
  - Their durations overlap
- Access is **instantaneous** if it’s not possible for other code to run after that access starts but before it ends
- **Long-term** accesses span the execution of other code, and can **overlap** with other access
- A **function** has **long-term write access** to all of its **in-out parameters** (so you can’t access the original variable that was passed as in-out)
- A **mutating method on a structure** has write access to **self** for the duration of the method call

### Access control

- Swift’s access control model: based on the concept of modules, source files, and packages
- A **module** is a single unit of code distribution 
- A **package** is a group of modules that you develop as a unit
- Swift provides six different access levels:
  - **open** & **public**: within defining module & other modules which import the defining module
  - **package**: within defining package
  - **internal**: within defining module
  - **file-private**: within defining source file
  - **private**: within the enclosing declaration & the extension of the declaration in the same file
- Overall guiding principle: **no entity can be defined in terms of another entity that has a lower (more restrictive) access level**

### Result builder

- SwiftUI是Swift内嵌的一种领域特殊语言DSL (DSL的核心目的：让你写代码时像是描述东西，而不是调用一堆API)
- **结果构建器** = 一组static方法 + 编译器自动把闭包里的代码翻译成对这些方法的调用 = 写起来像是“在闭包里堆一堆东西”，实际上是“编译器在帮你调用一堆静态方法，把这些东西合成一个最终结果”
- VStack: @ViewBuilder content: () -> Content

## Programming styles

### Functional programming 

- 高阶函数: filter等，将函数作为参数传入 (函数式编程的核心)

  _**filter**_ :

  <img src="/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260128150254690.png" alt="image-20260128150254690" style="zoom:50%;" />

- 强调：数据结构尽量不被修改，而是生成新的数据

- 重要风格：声明式写法，描述“是什么”而不是“怎么做”
