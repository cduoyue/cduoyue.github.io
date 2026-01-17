---
layout: post
title:  "Swift Core Fundamentals"
date:   2025-06-01 
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

  ![image-20260117140522808](/Users/zhengjiancheng/Library/Application Support/typora-user-images/image-20260117140522808.png)

- Capabilities of properties (computed property & static stored property), methods, initialization, extension, and protocols

### Class and structure 

- Property observer: willSet, didSet
- Property wrapper: syntactic sugar for a property with a getter and a setter

### Protocol

- Blue print

### Error handling

- Error protocol (empty)
- Four ways to handle: propagation, do-catch, try?, try!

## Memory management

### ARC (Automatic Reference Counting)

- Strong/weak reference: strong reference cycle, weak & unowned
- Closure retain cycle: closure capture list, unowned & weak

## Advanced features 

### Generics

- Functions and types
- Protocol: associatedType

### Extensions

### Higher order funcitons

- map, filter

## Standard library

### Common data structures

- Array, dictionary, set

### String manipulation 

### Date

### URL

### JSON parsing

- typealias Codable = Encodable & Decodable
- jsonData = try? JSONEncoder().encode(user), jsonString = String(data: jsonData, encoding: .utf8)
- jsonData = jsonString.data(using: .utf8), decodedUser = try? JSONDecoder().decode(User.self, from: jsonData)
- JSONEncoder, JSONDecoder -> Foundation framework

