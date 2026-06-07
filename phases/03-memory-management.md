[⬅ Back to Roadmap](../README.md)

# Phase 3 — Memory Management (ARC)

A guaranteed interview area for iOS. If you can confidently explain retain cycles and fix them live, you signal senior-level competence.

---

## 3.1 ARC — Automatic Reference Counting

**Summary (easy):** ARC counts how many strong references point to a class instance. When the count hits zero, the object is freed. It happens at compile time (retain/release calls inserted), not via a background garbage collector.

**Example:**
```swift
class Car { deinit { print("Car freed") } }
var a: Car? = Car()   // count = 1
var b = a              // count = 2
a = nil                // count = 1, not freed
b = nil                // count = 0 -> "Car freed"
```

**Interview angle:** ARC only applies to reference types (classes, closures). Structs/enums are value types — no ARC.

**Resources:** Apple — *Automatic Reference Counting*; WWDC "ARC and Memory".

---

## 3.2 strong / weak / unowned

**Summary (easy):** `strong` (default) keeps the object alive. `weak` does not, and becomes `nil` automatically when the object is freed (must be optional). `unowned` also doesn't keep it alive but assumes it'll never be nil (crashes if accessed after free).

**Example:**
```swift
class Person { var card: CreditCard? }
class CreditCard { unowned let owner: Person; init(o: Person) { owner = o } }
class Job { weak var employee: Person? }
```

**Interview angle:** Rule of thumb — use `weak` when the other object can outlive this one (delegates), `unowned` when it definitely can't (lifetimes tied together). Getting this choice right is the test.

**Resources:** Apple — *Resolving Strong Reference Cycles*; Swift by Sundell.

---

## 3.3 Retain Cycles (memory leaks)

**Summary (easy):** A retain cycle happens when two objects hold strong references to each other, so neither ever reaches count zero — they leak. Break the cycle by making one reference `weak`/`unowned`.

**Example:**
```swift
// Classic delegate leak (FIX: make delegate weak)
protocol Delegate: AnyObject {}
class Controller { weak var delegate: Delegate? }
```

**Interview angle:** You may be handed leaky code and asked to spot/fix it. Common culprits: delegates declared `strong`, timers, and closures capturing `self`.

**Resources:** Kodeco — "ARC and Memory Management"; SwiftLee — "Retain cycles".

---

## 3.4 Capture Lists in Closures ([weak self] / [unowned self])

**Summary (easy):** A closure stored on `self` that also references `self` creates a cycle. Add a capture list `[weak self]` to break it. Inside, `self` becomes optional.

**Example:**
```swift
class ViewModel {
    var onUpdate: (() -> Void)?
    func setup() {
        onUpdate = { [weak self] in
            guard let self else { return }
            self.refresh()
        }
    }
    func refresh() {}
}
```

**Interview angle:** Not *every* closure needs `[weak self]` — only escaping closures stored long-term. A `DispatchQueue.main.async` one-shot generally doesn't leak (it releases after running).

**Resources:** Swift by Sundell — "Capturing objects"; SwiftLee — "Weak self".

---

## 3.5 Value Semantics & Copy-on-Write (COW)

**Summary (easy):** Swift's standard collections (Array, Dictionary, String) behave like value types but don't copy their storage until you mutate a shared copy — that's copy-on-write, giving value-type safety with reference-type efficiency.

**Example:**
```swift
var a = [1, 2, 3]
var b = a        // shares buffer, no copy yet
b.append(4)      // now a copy is made (a stays [1,2,3])
```

**Interview angle:** You can implement COW for your own types with `isKnownUniquelyReferenced(_:)` — a great senior-level talking point.

**Resources:** Apple Swift blog — "Value and Reference Types"; objc.io "Advanced Swift" (COW chapter).

---

## 3.6 Stack vs Heap, autoreleasepool, Memory Graph Debugger

**Summary (easy):** Value types usually live on the stack (fast, auto-cleaned); class instances live on the heap (needs ARC). `autoreleasepool` drains temporary objects sooner inside tight loops. Xcode's Memory Graph and Instruments' Leaks tool find cycles.

**Example:**
```swift
for _ in 0..<100_000 {
    autoreleasepool { let img = makeBigImage() /* freed each loop */ }
}
func makeBigImage() -> Any { [UInt8](repeating: 0, count: 1_000_000) }
```

**Interview angle:** Know how to *demonstrate* finding a leak: Instruments → Leaks/Allocations, or the Memory Graph "purple !" warnings.

**Resources:** Apple — *Debugging memory issues with Instruments*; WWDC "Visual Debugging with Xcode".
