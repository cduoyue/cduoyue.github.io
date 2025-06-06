---
layout: post
title:  "SwiftUI Fundamentals"
date:   2025-06-06 00:00:00 +0000
tags: [iOS, SwiftUI]
author: Neoren
excerpt: "A comprehensive guide to SwiftUI fundamentals for iOS development"
---
## What is SwiftUI & How does it work?

- UI framework, introduced in 2019
- Concise code, compared to UIKit
- Declarative syntax (declares what you want the UI to display), more intuitive & efficient 
- Composable and reusable (build small, reusable UI components)
- Data-driven, keeping the UI and the data in sync
- Built-in animation and graphics 
- Common components: views, stacks, and modifiers
- Code easier to read and maintain
- Integrate a SwiftUI view into a UIKit-based app (using a _UIHostingController_)
- Consistent across Apple devices (one API)
- Faster iteration with previews
- Future of iOS UI development 

## SwiftUI Pathway

### Seek out the best view

- In your app, finding the right way to display information across different views is crucial to keeping your app clean and functional
- A range of container views: some purely for structure and layout (VStack, Grid), some also adopt system-standard visuals and interactivity (List, Form)
- Choosing the most appropriate container views is an important skill to learn 

### Take a structured approach to design layout

- Align any contained views inside a stack view by using a combination of the alignment property, Spacer, and Divider views
- Whenever possible, define structure and hierarchy rather than explicitly positioning view frames

### Plan for navigation

- Great navigation can go unnoticed; the easier your app is to interact with, the less you think about those interactions 

### Dive into data

- Data modeling
- “When SwiftUI looks at your code, what does it see?”

## SwiftUI tutorial

### General concepts

- Types of scenes: WindowGroup, Window, DocumentGroup, Settings
- Adding a border to a view is a great debugging tool

### Buttons and state

- View state: owned by the view, private
- Buttons, states, and closures: enable your app to respond to user actions, creating powerful and expressive interfaces

### Lists and text fields

- TextField, List, and bindings: create dynamic content

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

### Binding

- Property wrapper: @State, @Binding
- Let _count = State<Int> instance, then $count is a Binding<Int> fetched from State (both pointing to the same data source). When it is passed to a child view’s _myCount: Binding<Int>, then both count and myCount are binded to the same class instance

### Custom types

- Data modeling: the art of representing real-world concepts as models
- Data modeling includes: data & business logic (app-specific ways in which data is manipulated)

### Swift testing

- Automated tests, unit test

### Models and persistence

- @Model converts a Swift class into a stored model managed by SwiftData
- SwiftData uses ‘class’ because the identity of the class can make SwiftData uniquely identify and globally share the data object
- Data storage: ModelContainer, ModelContext, EnvironmentValues, @Environment(\\.keyPath), @Query

### Navigation

- Modal interface: focus people on a short, well defined task that they must complete or cancel before continuing
- Sheet: appear when an optional has a value
- A picker can appear as: a pop-up menu, a group of buttons, a separate view pushed onto the navigation stack

## SwiftUI elements

### Basic views

- Text(“Hello, world!”)
- Spacer()
- Divider()
- Image(systemName: “globe”), SF Symbols; Image(imageName)
- Button(“Roll”) {}, Button(“Remove Dice”, systemImage: “minus.circle.fill”) {}, Button {} label: { any View }, Button(action: ) {}
- ContentUnavailableView(“no movies”, systemImage: “film.stack”), ContentUnavailableView { Label(“no friends”, systemImage: “”) }
- TextField(_ titleKey: LocalizedStringKey, text: Binding<String>), TextField(“Add Name”, text: $nameToAdd).autocorrectionDisabled().onSubmit {Names.append(nameToAdd), nameToAdd = “”}
- Toggle(_ titleKey: LocalizedStringKey, isOn: Binding<Bool>)
- DatePicker(selection: Binding<Date>, label: () -> View) 
- ColorPicker(“color”, selection: $colors[0])
- Picker(“Favorite Movie”, selection: $friend.favoriteMovie) {}
- NavigationLink { MovieDetail(movie: movie) } label: { Text(movie.title) }, NavigationLink(_title: StringProtocol, destination: () -> View)
- ToolbarItem(placement: ToolbarItemPlacement.navigationBarTrailing) { EditButton() }, ToolbarItem(content: () -> View)
- Slider(value: $depth, in: 0…50) { Text(“Depth”) }
- RoundedRectangle(cornerRadius: 17)
- Tab(“Friends”, systemName: “person.and.person”) { Text(“Friends”) }

### Container views

- VStack {}, HStack {}, ZStack {}, VStack(alignment: HorizontalAlignment.leading, spacing: 20) {}
- ScrollView(Axis.Set.horizontal) {}
- List {}
- TabView {}.tabViewStyle(.page)
- NavigationStack {}
- NavigationSplitView {} detail: {}
- Group {}
- Section(“Favorited by”) {}
- Grid { GridRow {} }

### Modifiers

- Nature of modifiers:
  - Most modifiers are methods of the extended View protocol
  - These modifiers typically return a new view that wraps the original view, using some View as return type
  - This design allows us to chain multiple modifiers 
  - We can create our own modifiers 
  - Other modifiers can be methods of specific view type
- .imageScale(.large)
- .padding(), .padding(10), .padding(Edge.Set.horizontal, 20), .padding([.top, .leading], 20), .padding(EdgeInsets(top: 10, leading: 20, bottom: 10, trailing: 20))
- .background(Color.yellow, in: RoundedRectangle(cornerRadius: 12)), .background {}, .background(Gradient(colors: gradientColors)), .background(Color.appBackground), .background(Material.bar)
- .shadow(radius: 17), .shadow(color: Color.gray, radius: 17)
- .foregroundStyle(Color.yellow), .foregroundStyle(TintShapeStyle.tint), .foregroundStyle(Color.black, Color.white)
- .font(Font.headline), .font(Font.system(size: 78)), .font(Font.largeTitle.lowercaseSmallCaps()) 
- .fontWeight(Font.Weight.semibold), .fontDesign(Font.Design.monospaced), .fontWidth(Font.Width.compressed)
- .multilineTextAlignment(TextAlignment.center) 
- .border(Color.black, width: 1.7) 
- .resizable(), .frame(width: 170, height: 170), .frame(maxWidth: 100, maxHeight: 100), .frame(maxHeight: CGFloat.infinity), .frame(maxWidth: CGFloat.infinity, alignment: Alignment.trailing)
- .overlay(alignment: Alignment.topLeading) {}
- .rotationEffect(Angle.degrees(180))
- .tabViewStyle(PageTabViewStyle.page)
- .buttonStyle(BorderedButtonStyle.bordered)
- .buttonBorderShape(ButtonBorderShape.capsule)
- .disabled(numberOfDice == 1)
- .aspectRatio(1, contentMode: ContentMode.fit)
- .labelStyle(.iconOnly)
- .tint(Color.white)
- .symbolRenderingMode(SymbolRenderingMode.multicolor)
- .autocorrectionDisabled()
- .bold(), .bold(true)
- .clipShape(RoundedRectangle(cornerRadius: 8))
- .focused($isTextFieldFocused) (.focused(_ condition: FocusState<Bool>.Binding))
- .onSubmit {} (.onSubmit(action: () -> Void))
- .onAppear {}
- .contentShape(Rectangle())
- .onTapGesture {}
- .navigationTitle(“Birthdays”)
- .navigationBarTitleDisplayMode(.inline)
- .safeAreaInset(edge: VerticalEdge.bottom) {}
- .textFieldStyle(RoundedBorderTextFieldStyle.roundedBorder)
- .modelContainer(for: Book.self)
-  .gesture(DragGesture().onChanged { value in })
- .tabItem { Label(“Movies”, systemImage: “film.stack”) } 
- .onDelete(perform: deleteItems) (.onDelete(perform: Optional<(IndexSet) -> Void>))
- .onMove { indices, newOffset in players.move(fromOffsets: indices, toOffset: newOffset) }
- .toolbar { toolbarItem() {}, toolbarItem {} }, .toolbar { Button() } (syntax sugar)
- .sheet(item: $newMovie) { movie in NavigationStack { MovieDetail(movie: movie, isNew: true) } }
- .tag()
- .searchable(text: $searchText)
- .labelsHidden()
- .stroke(lineWidth: 24), .strokeBorder(lineWidth: 24)
- .clipShape(Circle())
- .offset(y: -130)
- .gridColumnAlignment(HorizontalAlignment.leading）
- .controlSize(ControlSize.large)
- .accessibilityElement(children: AccessibilityChildBehavior.ignore)
- .accessibilityLabel(“time remaining”)
- .accessibilityValue(“10 minutes”)

### ForEach view

- ForEach(1…numberOfDice, id: \.description) { _ in DiceView() }
- ForEach(names, id: \.description) { name in Text(name) }
- ForEach(movies) { movie in } (Movie: Identifiable)
- ForEach(0..<4) { index in Circle() } (use convenience initializer)
- ForEach($players) { $player in TextField("Name", text: $player.name) }

### Date

- Calendar.current.isDateInToday(birthday)
- Date(timeIntervalSinceReferenceDate: -876_000_000)

### Global functions

- func withAnimation<Result>(_ animation: Animation? = .default, _ body: () throws -> Result) rethrows -> Result, animation can be static property, static methods or chain instance methods

### Special syntax

- var id = UUID(), universally unique identity, from Foundation, UUID() -> generating a random 128 bits number

## SwiftData

### @Model

- A macro
- Managed by SwiftData

### .modelContainer(for Pal.self)

- Configure data model
- Create ModelContainer instance
- Put the container into the environment, accessed by all the child views

### @Environment(\\.modelContext) private var context

- Gain the ModelContext instance from the environment, used for data management

### @Query(sort: \\Pal.name) private var pals: [Pal]

- Gain all Pal objects from SwiftData, sorted by name property
