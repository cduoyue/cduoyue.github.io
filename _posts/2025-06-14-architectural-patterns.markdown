---
layout: post
title:  "Architectural Patterns"
date:   2025-06-14 00:00:00 +0000
tags: [iOS, Software Architecture Pattern]
author: Neoren
excerpt: "A comprehensive guide to MVVM for iOS development"
---
## Trends in the usage of mainstream architectural patterns 

### MVVM (Model-View-ViewModel)

- Very popular in SwiftUI development & is one of the most commonly used architectural patterns among individual developers
- SwiftUI’s data-driven mechanism such as @State and @ObservableObject are fundamentally well-suited to MVVM
- Overall, MVVM still holds a dominant position in the SwiftUI era

### MVC (Model-View-Controller)

- For simple applications, many individual developers adopt a model-view direct-binding approach; this can be seen as a lightweight MVC or no-architecture pattern in SwiftUI
- Business logic directly embedded into SwiftUI Views or models for rapid development 
- When the app becomes more complex, it can lead to bloated views

### TCA (The Composable Architecture)

- Recently quickly gaining popularity in the SwiftUI community 
- Introduced by the Point-Free team
- Increasingly gaining attention among individual developers, especially for apps of a certain scale and with complex business logic
- A relatively steep learning curve, and integrating a third-party library can also introduce coupling issues
- Powerful testability and modularity have attracted many experienced individual developers to try it
- If a project requires complex state sharing or a strict unidirectional data flow, individual developers often consider adopting TCA
- As of 2024, TCA has garnered over 12 k stars on GitHub, becoming one of the most popular architecture libraries in the Swift community
- The overall trend is that for personal projects aiming for code robustness and scalability, the adoption rate of TCA is on the rise

## Analysis of architectural patterns and their compatibility with SwiftUI

### MVVM

- Core idea: MVVM separates the user interface from business state, with the ViewModel responsible for view state management and business logic, while the Model represents the data model
- Each SwiftUI view typically corresponds to a ViewModel, which helps keep the view simple and pure
- GitHub: todor app & Steps app & MovieSwiftUI
- This architecture emphasizes that each view has a corresponding ViewModel, separating view state and user inputs, simplifying the interaction between views and data, and enhancing component reusability



