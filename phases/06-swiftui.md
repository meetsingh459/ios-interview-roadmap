[⬅ Back to Roadmap](../README.md)

# Phase 6 — SwiftUI

Most new iOS work is SwiftUI, so interviewers test data flow and how it differs from UIKit's imperative model. The state-management property wrappers are the heart of it.

---

## 6.1 Declarative UI, the View protocol & body

**Summary (easy):** In SwiftUI you *describe* what the UI should look like for a given state, and the framework figures out the changes. A `View` is a value type with a `body` that returns more views.

**Example:**
```swift
struct Greeting: View {
    let name: String
    var body: some View { Text("Hi, \(name)") }
}
```

**Interview angle:** Explain "declarative vs imperative." Views are cheap structs recreated often — never put expensive work in `body`.

**Resources:** Apple — *SwiftUI Tutorials*; Hacking with Swift — "100 Days of SwiftUI".

---

## 6.2 @State & @Binding

**Summary (easy):** `@State` is a view's own private, mutable source of truth (value types). `@Binding` is a *reference* to state owned elsewhere, letting a child read/write the parent's value.

**Example:**
```swift
struct Parent: View {
    @State private var on = false
    var body: some View { Toggle("On", isOn: $on) }  // $on is a Binding
}
```

**Interview angle:** `$` gives the projected value (a Binding). Use `@State` for simple, local, value-type state only.

**Resources:** Apple — *Managing User Interface State*; Hacking with Swift state articles.

---

## 6.3 @StateObject vs @ObservedObject vs @EnvironmentObject

**Summary (easy):** For class-based view models conforming to `ObservableObject`: `@StateObject` creates and *owns* the object (survives view redraws); `@ObservedObject` references one created elsewhere; `@EnvironmentObject` reads one injected into the environment.

**Example:**
```swift
class VM: ObservableObject { @Published var count = 0 }
struct Screen: View {
    @StateObject private var vm = VM()   // owner creates it here
    var body: some View { Text("\(vm.count)") }
}
```

**Interview angle:** The classic bug: using `@ObservedObject` to *create* a view model → it gets recreated on each redraw and loses state. Use `@StateObject` to own it.

**Resources:** Apple docs; SwiftLee — "StateObject vs ObservedObject".

---

## 6.4 The Observation framework (@Observable, iOS 17+)

**Summary (easy):** `@Observable` (the macro-based Observation framework) replaces `ObservableObject`/`@Published`. Views automatically track only the properties they actually read, improving performance and removing boilerplate.

**Example:**
```swift
@Observable class VM { var count = 0 }
struct Screen: View {
    @State private var vm = VM()        // @State now holds the @Observable
    var body: some View { Text("\(vm.count)") }
}
```

**Interview angle:** Know *why* it's better: fine-grained dependency tracking → fewer view updates than `@Published` (which invalidates on any change).

**Resources:** WWDC23 "Discover Observation in SwiftUI"; Apple — *Observation*.

---

## 6.5 @Environment & Dependency Injection in SwiftUI

**Summary (easy):** `@Environment` reads system or custom values passed down the view tree (color scheme, locale, dismiss action, custom keys). It's SwiftUI's built-in dependency injection.

**Example:**
```swift
struct Row: View {
    @Environment(\.colorScheme) var scheme
    @Environment(\.dismiss) var dismiss
    var body: some View { Button("Close") { dismiss() } }
}
```

**Interview angle:** Know how to define a custom `EnvironmentKey` to inject a service app-wide.

**Resources:** Apple — *Environment*; Point-Free dependency injection discussions.

---

## 6.6 Layout System (stacks, GeometryReader, alignment, layout protocol)

**Summary (easy):** SwiftUI lays out by a parent proposing a size, the child choosing its size, and the parent positioning it. `HStack/VStack/ZStack` stack views; `GeometryReader` exposes the available space; the `Layout` protocol (iOS 16) builds custom layouts.

**Example:**
```swift
GeometryReader { geo in
    Rectangle().frame(width: geo.size.width * 0.5)
}
```

**Interview angle:** Explain the "parent proposes, child decides" model — it's the key to debugging unexpected sizes.

**Resources:** WWDC22 "Compose custom layouts with SwiftUI"; objc.io "Thinking in SwiftUI".

---

## 6.7 Lists, ForEach & Identifiable

**Summary (easy):** `List` and `ForEach` render collections. Each element needs a stable identity (via `Identifiable` or a `id:` key path) so SwiftUI can animate insert/move/delete correctly.

**Example:**
```swift
List(items) { item in Text(item.title) }   // items: [Item] where Item: Identifiable
```

**Interview angle:** Using array indices as IDs causes glitches when the list mutates — explain why stable IDs matter.

**Resources:** Apple — *Lists*; Hacking with Swift list articles.

---

## 6.8 Navigation (NavigationStack & NavigationPath)

**Summary (easy):** `NavigationStack` (iOS 16+) drives push/pop with a data-driven path, enabling programmatic and deep navigation. `navigationDestination` maps a value type to a destination view.

**Example:**
```swift
NavigationStack(path: $path) {
    List(items) { NavigationLink(item.name, value: item) }
        .navigationDestination(for: Item.self) { DetailView($0) }
}
```

**Interview angle:** Contrast with deprecated `NavigationView`. Know how to push programmatically by appending to `path`.

**Resources:** WWDC22 "The SwiftUI cookbook for navigation"; Apple docs.

---

## 6.9 Modifiers, Animations & Transitions

**Summary (easy):** View modifiers return a new modified view (order matters!). Animations are declarative — change state inside `withAnimation` and SwiftUI interpolates; transitions animate insertion/removal.

**Example:**
```swift
Text("Hi").padding().background(.blue)   // order matters
withAnimation(.spring) { isExpanded.toggle() }
```

**Interview angle:** "Why does `.padding().background()` differ from `.background().padding()`?" — modifier ordering wraps views in layers.

**Resources:** Apple — *Animations*; Hacking with Swift animation guides.

---

## 6.10 UIKit ↔ SwiftUI Interoperability

**Summary (easy):** Wrap UIKit views in SwiftUI with `UIViewRepresentable`/`UIViewControllerRepresentable`; embed SwiftUI in UIKit with `UIHostingController`. Essential for incremental migration.

**Example:**
```swift
struct MapViewWrapper: UIViewRepresentable {
    func makeUIView(context: Context) -> MKMapView { MKMapView() }
    func updateUIView(_ v: MKMapView, context: Context) {}
}
```

**Interview angle:** Real teams migrate gradually — expect "how would you embed an existing UIKit screen in a new SwiftUI app?".

**Resources:** Apple — *UIViewRepresentable*; WWDC "Use SwiftUI with UIKit".
