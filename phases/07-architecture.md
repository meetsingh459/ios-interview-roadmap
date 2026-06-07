[⬅ Back to Roadmap](../README.md)

# Phase 7 — Architecture & Design Patterns

Senior interviews lean heavily here: "How would you structure this screen/feature?" You need opinions backed by trade-offs, not just definitions.

---

## 7.1 MVC, MVP, MVVM, VIPER, Clean, TCA

**Summary (easy):** These are ways to split responsibilities so code stays testable and maintainable. MVC (Apple's classic, often "Massive VC"), MVP (presenter holds logic), MVVM (view model exposes state — pairs perfectly with SwiftUI/Combine), VIPER (very granular), Clean Architecture (layered use-cases), TCA (unidirectional, state-driven).

**Example (MVVM idea):**
```swift
@Observable class ProfileVM {     // view model owns state + logic
    var user: User?
    func load() async { user = try? await api.fetchUser() }
}
```

**Interview angle:** Don't just name them — say *when* you'd choose each and the cost (VIPER = many files/boilerplate; MVVM = great default for SwiftUI). Explain how each improves testability.

**Resources:** Kodeco — iOS architecture series; Point-Free — TCA; objc.io "App Architecture" book.

---

## 7.2 Delegation Pattern

**Summary (easy):** One object hands off responsibility to another via a protocol. Used everywhere in UIKit (`UITableViewDelegate`). The delegate is usually `weak` to avoid retain cycles.

**Example:**
```swift
protocol PickerDelegate: AnyObject { func didPick(_ value: String) }
class Picker { weak var delegate: PickerDelegate? }
```

**Interview angle:** Delegation = one-to-one callback. Contrast with closures (lighter) and NotificationCenter (one-to-many).

**Resources:** Apple — *Delegation*; Swift by Sundell.

---

## 7.3 Observer Pattern: NotificationCenter & KVO

**Summary (easy):** Lets objects react to events without tight coupling. `NotificationCenter` broadcasts named notifications to many listeners; KVO (Key-Value Observing) watches a specific property for changes.

**Example:**
```swift
NotificationCenter.default.addObserver(
    forName: .didLogin, object: nil, queue: .main) { _ in refresh() }
```

**Interview angle:** Know KVO requires `@objc dynamic` properties (Obj-C runtime). Modern Swift prefers Combine/Observation over both.

**Resources:** Apple — *NotificationCenter*, *KVO*.

---

## 7.4 Singleton (and its pitfalls)

**Summary (easy):** A single shared instance for the whole app (`URLSession.shared`, `UserDefaults.standard`). Convenient but easy to abuse — hidden global state hurts testability.

**Example:**
```swift
final class Analytics { static let shared = Analytics(); private init() {} }
```

**Interview angle:** Be critical: singletons make unit testing hard (can't inject a fake). Prefer dependency injection; use singletons sparingly and behind a protocol.

**Resources:** Swift by Sundell — "Avoiding singletons"; Point-Free episodes.

---

## 7.5 Dependency Injection (DI)

**Summary (easy):** Instead of an object creating its own dependencies, you pass them in (constructor, property, or method). This makes code testable and flexible.

**Example:**
```swift
protocol APIClient { func fetch() async -> Data }
final class Feed {
    let api: APIClient
    init(api: APIClient) { self.api = api }   // injected -> mockable in tests
}
```

**Interview angle:** Constructor injection is preferred. Be able to explain how DI enables swapping a real API for a mock in tests.

**Resources:** Swift by Sundell — "Dependency injection"; Point-Free "Dependencies".

---

## 7.6 Coordinator Pattern

**Summary (easy):** Moves navigation logic out of view controllers into a dedicated "coordinator" object, so screens don't need to know about each other. Improves reuse and testability of flows.

**Example:**
```swift
final class AppCoordinator {
    let nav: UINavigationController
    func start() { nav.setViewControllers([HomeVC(coordinator: self)], animated: false) }
    func showDetail() { nav.pushViewController(DetailVC(), animated: true) }
}
```

**Interview angle:** Solves "view controllers shouldn't instantiate each other." Common in UIKit MVVM-C.

**Resources:** Hacking with Swift — "Coordinator pattern"; Soroush Khanlou's original articles.

---

## 7.7 Factory, Builder, Adapter, Strategy, Repository

**Summary (easy):** Reusable solutions to recurring problems: Factory (create objects without exposing concrete types), Builder (step-by-step construction), Adapter (make incompatible interfaces work together), Strategy (swap algorithms behind a protocol), Repository (abstract data sources behind one interface).

**Example (Strategy):**
```swift
protocol SortStrategy { func sort(_ a: [Int]) -> [Int] }
struct QuickSort: SortStrategy { func sort(_ a: [Int]) -> [Int] { a.sorted() } }
```

**Interview angle:** You don't need all 23 GoF patterns — know the 5–6 above with a Swift example each and when each appears in iOS code.

**Resources:** "Design Patterns by Tutorials" (Kodeco); refactoring.guru (language-agnostic).

---

## 7.8 SOLID Principles & Composition over Inheritance

**Summary (easy):** SOLID = five guidelines for maintainable OOP: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion. "Composition over inheritance" means build behavior by combining small pieces (protocols/structs) rather than deep class trees.

**Example (Dependency Inversion):**
```swift
// High-level code depends on an abstraction, not a concrete DB
protocol Storage { func save(_ s: String) }
final class Logger { let storage: Storage; init(storage: Storage) { self.storage = storage } }
```

**Interview angle:** Tie SOLID to real refactors. "Single Responsibility" is the answer to "how do you avoid a Massive View Controller?"

**Resources:** Uncle Bob's SOLID articles; Kodeco SOLID-in-Swift tutorial.
