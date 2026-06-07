[⬅ Back to Roadmap](../README.md)

# Phase 14 — Advanced & Niche Topics

The "senior signal" round. You won't get all of these, but being able to discuss a few with confidence sets SDE-3 candidates apart.

---

## 14.1 Method Dispatch (static, dynamic, witness table, vtable)

**Summary (easy):** Dispatch decides which implementation of a method runs. Static (direct, fastest) for `final`/value types; vtable (table lookup) for class inheritance; witness table for protocol methods; dynamic (Obj-C `objc_msgSend`) for `@objc dynamic`.

**Example:**
```swift
final class A { func f() {} }     // static dispatch
class B { func g() {} }           // vtable dispatch (overridable)
protocol P { func h() }            // witness table dispatch
```

**Interview angle:** This is a hallmark senior question. Know how `final`, `private`, and value types let the compiler devirtualize for speed.

**Resources:** WWDC16 "Understanding Swift Performance"; Swift.org optimization notes.

---

## 14.2 Objective-C Interop (@objc, dynamic, bridging)

**Summary (easy):** Swift and Obj-C coexist. `@objc` exposes Swift to the Obj-C runtime (needed for selectors, KVO, some frameworks); `dynamic` forces runtime dispatch. A bridging header imports Obj-C into Swift.

**Example:**
```swift
@objc func handleTap() {}   // usable with #selector
@objc dynamic var progress = 0.0   // KVO-observable
```

**Interview angle:** Know *when* you still need `@objc` (selectors, KVO, NSCoding, some legacy APIs) and the cost of dynamic dispatch.

**Resources:** Apple — *Importing Objective-C into Swift*; Swift/Obj-C interop guide.

---

## 14.3 Method Swizzling & the Runtime

**Summary (easy):** Swizzling swaps two methods' implementations at runtime using the Obj-C runtime. Powerful (analytics hooks, debugging) but risky — global, order-dependent, easy to break. Use sparingly.

**Example (concept):**
```swift
method_exchangeImplementations(original, replacement)
```

**Interview angle:** Be able to explain a legitimate use (e.g., tracking `viewDidAppear` app-wide) *and* why it's dangerous and discouraged in app code.

**Resources:** NSHipster — "Method Swizzling"; Apple Obj-C runtime reference.

---

## 14.4 Result Builders (@resultBuilder)

**Summary (easy):** A result builder transforms a block of statements into a single value — the magic behind SwiftUI's `body` (`ViewBuilder`). You can build your own DSLs.

**Example:**
```swift
@resultBuilder struct StringBuilder {
    static func buildBlock(_ parts: String...) -> String { parts.joined(separator: " ") }
}
```

**Interview angle:** Know that SwiftUI's declarative syntax is powered by `@resultBuilder` + trailing closures + opaque return types.

**Resources:** Swift Evolution SE-0289; Apple — *Result builders*; Hacking with Swift.

---

## 14.5 Macros (Swift 5.9+)

**Summary (easy):** Macros generate code at compile time from an annotation, reducing boilerplate while staying type-safe. `@Observable`, `#Preview`, and SwiftData's `@Model` are macros. Two kinds: freestanding (`#name`) and attached (`@name`).

**Example:**
```swift
@Observable class Counter { var value = 0 }   // expands to observation plumbing
let url = #URL("https://apple.com")            // compile-time validated (example)
```

**Interview angle:** You won't be asked to *write* a macro often, but explaining what they are and naming examples shows you're current.

**Resources:** WWDC23 "Expand on Swift macros"; Apple — *Macros*.

---

## 14.6 Modules, Frameworks & Swift Package Manager

**Summary (easy):** Code is organized into modules; you can package reusable code as static or dynamic frameworks/libraries. SPM is Apple's dependency manager (`Package.swift`) — now the default over CocoaPods/Carthage.

**Example:**
```swift
// Package.swift
.target(name: "Networking", dependencies: [])
```

**Interview angle:** Know static vs dynamic linking trade-offs (launch time vs binary size), and modularization benefits (build times, ownership, testability).

**Resources:** Apple — *Swift Packages*; WWDC SPM sessions.

---

## 14.7 Accessibility (VoiceOver, Dynamic Type)

**Summary (easy):** Accessibility makes your app usable by everyone: VoiceOver reads the screen, Dynamic Type scales fonts, and proper labels/traits describe controls. It's increasingly expected (and asked) at top companies.

**Example:**
```swift
button.accessibilityLabel = "Play"
Text("Hi").dynamicTypeSize(...DynamicTypeSize.accessibility3)   // SwiftUI
```

**Interview angle:** Know `accessibilityLabel/value/traits`, supporting Dynamic Type, and why hard-coded font sizes are an anti-pattern.

**Resources:** Apple — *Accessibility*; WWDC accessibility sessions.

---

## 14.8 Localization, Security & Reflection

**Summary (easy):** Localization adapts text/formats per region (`Localizable.strings`, `String(localized:)`, format-aware dates/numbers). Security includes Data Protection (file encryption classes), avoiding storing secrets in code, and the App Sandbox. `Mirror` gives limited runtime reflection.

**Example:**
```swift
let greeting = String(localized: "welcome_message")
let mirror = Mirror(reflecting: someStruct)   // inspect properties
```

**Interview angle:** Know that Swift reflection is read-only/limited (unlike Java). For security, know Data Protection levels and that sensitive data belongs in Keychain.

**Resources:** Apple — *Localization*, *Encrypting Your App's Files*, *Mirror*; OWASP Mobile Top 10.
