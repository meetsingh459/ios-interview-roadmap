[⬅ Back to Roadmap](../README.md)

# Phase 2 — Intermediate Swift & The Type System

This is where SDE-2/3 interviews start separating people. Protocols, generics, and the `some`/`any` distinction come up constantly.

---

## 2.1 Protocols & Protocol-Oriented Programming (POP)

**Summary (easy):** A protocol is a contract: "any type that conforms must provide these methods/properties." POP means building behavior by composing small protocols instead of deep class inheritance.

**Example:**
```swift
protocol Drawable { func draw() }
struct Circle: Drawable { func draw() { print("○") } }
struct Square: Drawable { func draw() { print("□") } }
let shapes: [Drawable] = [Circle(), Square()]
shapes.forEach { $0.draw() }
```

**Interview angle:** "Protocol vs base class?" → protocols allow multiple conformance, work with value types, and avoid the fragile-base-class problem.

**Resources:** Apple — *Protocols*; WWDC "Protocol-Oriented Programming in Swift"; Hacking with Swift — "POP".

---

## 2.2 Protocol Extensions & Default Implementations

**Summary (easy):** You can add default method implementations to a protocol via an extension, so conformers get behavior for free unless they override it.

**Example:**
```swift
protocol Greeter { func greet() }
extension Greeter { func greet() { print("Hello") } }  // default
struct Robot: Greeter {}   // uses default greet()
```

**Interview angle:** Watch the gotcha: static vs dynamic dispatch. A method only declared in the extension (not the protocol requirement) is dispatched statically — calling it through the protocol type uses the extension version, not an override.

**Resources:** Apple — *Protocol Extensions*; Swift by Sundell — "Protocol extension dispatch".

---

## 2.3 Generics (constraints, where clauses)

**Summary (easy):** Generics let you write one function/type that works with many types while staying type-safe. Constraints restrict which types are allowed.

**Example:**
```swift
func swapTwo<T>(_ a: inout T, _ b: inout T) { (a, b) = (b, a) }
func largest<T: Comparable>(_ items: [T]) -> T? { items.max() }
func indexOf<T>(_ x: T, in arr: [T]) -> Int? where T: Equatable {
    arr.firstIndex(of: x)
}
```

**Interview angle:** Generics are resolved at compile time (monomorphization) → fast. Contrast with `any` existentials which add a runtime box.

**Resources:** Apple — *Generics*; Hacking with Swift — "Generics".

---

## 2.4 Associated Types

**Summary (easy):** A protocol can declare a placeholder type (`associatedtype`) that each conformer fills in. This is how `Collection`, `Sequence`, etc. stay generic.

**Example:**
```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
}
struct IntBox: Container {
    var items = [Int]()
    mutating func append(_ item: Int) { items.append(item) }
    var count: Int { items.count }
}
```

**Interview angle:** Protocols with associated types ("PATs") can't be used as plain types pre-Swift 5.7 without `any`; know why.

**Resources:** Apple — *Associated Types*; Swift Evolution SE-0309 (existentials).

---

## 2.5 Opaque Types (`some`) vs Existentials (`any`)

**Summary (easy):** `some Protocol` = "one specific concrete type the compiler knows, but the caller doesn't" (zero overhead). `any Protocol` = "any type conforming, boxed at runtime" (flexible, slight cost).

**Example:**
```swift
func makeShape() -> some Drawable { Circle() }   // opaque: always Circle
let things: [any Drawable] = [Circle(), Square()] // existential: mixed
```

**Interview angle:** This is a *very* common modern Swift question. `some` keeps type identity (good for SwiftUI `body`), `any` erases it for heterogeneous collections.

**Resources:** Apple — *Opaque and Boxed Types*; Swift by Sundell — "some vs any"; WWDC22 "Embrace Swift generics".

---

## 2.6 Property Wrappers

**Summary (easy):** A property wrapper bundles get/set logic so you can reuse it with `@MyWrapper`. SwiftUI's `@State`, `@Published` are property wrappers.

**Example:**
```swift
@propertyWrapper struct Clamped {
    private var value = 0
    var wrappedValue: Int {
        get { value }
        set { value = min(max(newValue, 0), 100) }
    }
}
struct Volume { @Clamped var level: Int }
```

**Interview angle:** Know `wrappedValue` and `projectedValue` (`$`). Explains how `@Published`'s `$` exposes a publisher.

**Resources:** Apple — *Property Wrappers*; SwiftLee — "Property wrappers in Swift".

---

## 2.7 Codable (Encoding & Decoding)

**Summary (easy):** Conform to `Codable` and Swift auto-generates JSON↔model conversion. Customize with `CodingKeys`, custom `init(from:)`, and date/key strategies.

**Example:**
```swift
struct User: Codable {
    let id: Int
    let fullName: String
    enum CodingKeys: String, CodingKey { case id, fullName = "full_name" }
}
let user = try JSONDecoder().decode(User.self, from: jsonData)
```

**Interview angle:** Be ready for nested keys, snake_case mapping, decoding heterogeneous JSON, and `keyDecodingStrategy = .convertFromSnakeCase`.

**Resources:** Apple — *Encoding and Decoding Custom Types*; Swift by Sundell — "Codable" series.

---

## 2.8 Equatable, Hashable, Comparable, Identifiable

**Summary (easy):** These protocols let your types be compared (`==`), put in Sets/Dictionaries (`Hashable`), sorted (`Comparable`), and used in SwiftUI lists (`Identifiable`).

**Example:**
```swift
struct Person: Hashable, Comparable, Identifiable {
    let id = UUID()
    let name: String
    static func < (l: Person, r: Person) -> Bool { l.name < r.name }
}
```

**Interview angle:** Synthesized conformance only works when all stored properties conform. Know when you must implement `hash(into:)` manually.

**Resources:** Apple — *Equatable / Hashable*; Hacking with Swift articles.

---

## 2.9 Type Casting & `Any`/`AnyObject`

**Summary (easy):** `is` checks a type; `as?` tries a cast (returns optional); `as!` force-casts (crashes if wrong); `as` is for guaranteed/upcasts. `Any` holds anything; `AnyObject` holds any class.

**Example:**
```swift
let items: [Any] = [1, "two", 3.0]
for item in items {
    if let n = item as? Int { print("int \(n)") }
    else if item is String { print("a string") }
}
```

**Interview angle:** Prefer `as?` with binding; avoid `as!`. Discuss why overusing `Any` defeats Swift's type safety.

**Resources:** Apple — *Type Casting*.

---

## 2.10 Extensions, Computed Properties & Observers

**Summary (easy):** Extensions add functionality to existing types (even ones you don't own). Computed properties calculate on the fly; `willSet`/`didSet` observe changes; `lazy` defers creation until first use.

**Example:**
```swift
extension Int { var squared: Int { self * self } }
class Profile {
    var name = "" { didSet { print("changed to \(name)") } }
    lazy var avatar = expensiveLoad()
    func expensiveLoad() -> String { "img" }
}
```

**Interview angle:** `lazy` can't be `let`, isn't thread-safe, and runs once. `didSet` doesn't fire during initialization.

**Resources:** Apple — *Extensions* and *Properties*.
