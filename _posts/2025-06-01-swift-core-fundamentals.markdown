---
layout: post
title:  "Swift Core Fundamentals"
date:   2025-06-01 
tags: [iOS, Swift]
author: Neoren
---
## History

- Swift was born in WWDC 2014 & got open source license in 2015.
- Almost all frameworks are closed.

## Core syntax

### Basic types

- String: 21-bit unicode scalar, UTF-8/16/32, String.Index
- Int, Double, Bool, Array, Dictionary, Set, Optional
- Array syntax sugar: Array<Int>(arrayLiteral: 1, 2, 3) == Array<Int>([1, 2, 3]) == [1, 2, 3]
- Variable, constant

### Control flow

- if-else, switch
- for-in, while, repeat while

### Function and closure 

- Function: reference type
- Closure: light weighted syntax, capture values from surrounding, escape closure, variable boxing, reference type

### Class and structure 

- Property observer: willSet, didSet
- Property wrapper: syntactic sugar for a property with a getter and a setter

### Enumeration

- Capabilities of properties, methods, initialization, extension, and protocols
- CaseIterable, AllCases, allCases

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

