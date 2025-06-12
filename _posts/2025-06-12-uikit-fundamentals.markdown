---
layout: post
title:  "UIKit Fundamentals"
date:   2025-06-12 00:00:00 +0000
tags: [iOS, UIKit]
author: Neoren
excerpt: "A comprehensive guide to UIKit fundamentals for iOS development"
---
## What's UIKit & How does it work?

- Primary UI framework on iPhone, iPad
- Introduced in 2008, C++
- Remains essential and widely used due to its maturity and rich feature set
- Main components: UIView, UIViewController
- Imperative programming model:
  - Code that instructs how to do things, step by step 
  - Have fine-grained control over the UI
  - Code becomes verbose for large interfaces, and you must be careful to keep the code in sync
- Event-driven architecture:
  - The flow of your app is determined by events
  - When you start an app, it sets up a main event loop that continuously waits for events and dispatches them to your app's objects to handle
  - After you app launches and shows the first screen, it sits idle until something happens
  - UIKit processes events one at a time
- Auto Layout:
  - A constraint-based layout system
  - If building UI in code, you use _layout anchor APIs_ or _constraint objects_ to set the same rules
  - If there's a conflict, low-priority constraints can be broken to satisfy higher-priority ones

## UIKit introduction

### UIViewController

- View controller plays the role of managing the UI
- UIViewController is the base class of all view controllers
- Every view controller has a main view (the view property), on which many subviews can be added
- A view controller is responsible for coordinating the display and update of these views, responding to user interactions on the interface, and managing the transition from one screen to another 
- A view controller has a series of lifecycle methods, where we can write code to initialize the interface or refresh data

### Event handling (touches, gestures)

- When the user interacts with the screen, UIKit generates corresponding events (UIEvent) and delivers them to the app
- UIKit provided gesture recognizer (UIGestureRecognizer): UITapGestureRecognizer, UISwipeGestureRecognizer, UIPinchGestureRecognizer, UILongPressGrestureRecognizer
- When using gesture recognizers, we first create a gesture recognizer object, set the type of gesture it should detect, then attach it to a view and specify a callback method

### Navigation & pages switching

- Most commonly used: UINavigationController & modal view presenting
- The navigation controller manages a stack of view controllers
- navigationController?.pushViewController(detailVC, animated: true), navigationController?.popViewController(animated: true)
- Modal presentation: present(..., animated:completion:), dismiss(animated: completion:)

### Simple animation & transition effect

- We only need to tell the framework "I want to change the value of this property from A to B, using a certain animation, lasting for a certain duration"
- Most commonly used method: UIView.animate(withDuration:animation:), writing the desired final property value changes inside the animation closure
- More powerful animation interface: Core Animation

## Dev-doc UIKit

### About app development with UIKit

- App bundle: MyApp.app, a special folder including MyApp, Info.plist, Main.storyboard, Assets.car, Frameworks

- Info.plist file: declare your app's hardware and software requirements, the App Store prevents an app from being installed on a device that does not meet your app's requirements 

- MVC:

  ![MVC](https://github.com/cduoyue/cduoyue.github.io/blob/main/photos/MVC.png?raw=true)

- The UIApplication object has two core responsibilities:

  - Continuously listening for and dispatching user events to the appropriate objects
  - Handling state transitions such as launching, activating, backgrounding, and terminating, and calling the corresponding delegate methods 

### App and environment 

- Adapt to different environments by checking view.traitCollection or viewController.traitCollection

- The application(_:didFinishLaunchingWithOptions:) method lets you make final adjustments before the interface appears

- Performing long-running tasks in application(\_:didFinishLaunchingWithOptions:) or application(\_:willFinishLaunchingWithOptions:) might make your app appear sluggish to the user

- Launch options dictionary: when tapping App to launch, the dictionary will be empty or nil

- Launch sequence:

  ![launch sequence](https://github.com/cduoyue/cduoyue.github.io/blob/main/photos/Launch%20sequence.png?raw=true)

- At appropriate times, UIKit preserves the state of your app's views and view controllers to an encrypted file on disk. When your app is terminated  and relaunched later, UIKit reconstructs your views and view controllers from the preserved data

- Every iOS app has exactly one instance of UIApplication, which can be referred to as UIApplication.shared

- The UIApplication class defines a delegate that conforms to the UIApplicationDelegate protocol and must implement some of the protocol's methods

### Views and controls 

- The inheritance hierarchy: NSObject -> UIResponder -> UIView -> UIControl
- Frame (tells superview) & bounds (relates to subview)
- All UI operations: on main thread
- Collection views are a collaboration between many different objects: cells, layouts, UICollectionViewDataSource (protocol), UICollectionViewDelegate (protocol), UICollectionViewController

### View controllers

- Every view controller needs to do the following:
  - Fill its views with data, and update those views when data changes
  - Report data changes back to your data model objects
  - Adjust the size, position, and visibility of views to match the current environment 
  - Facilitate transitions to other pages of content
- Most custom view controllers you create are content view controllers -- that is, the view controller owns all of its views and manages interactions with those views
- UIViewController contains a content view, accessible from the view property, which serves as the root view of its view hierarchy 

