[⬅ Back to Roadmap](../README.md)

# Phase 1 — Swift Language Fundamentals

The bedrock. Interviewers assume you know this cold, so they use it to probe depth ("why a struct here?", "what happens if this is nil?"). Don't skip it just because it looks basic.

---

## 1.1 Variables, Constants & Type Inference

**Summary (easy):** `var` makes a value you can change, `let` makes one you can't. Swift usually figures out the type for you (type inference), but you can also write it explicitly.

**Example:**
```swift
let name = "Aman"        // inferred String, cannot change
var age = 30             // inferred Int, can change
let pi: Double = 3.14159 // explicit type
```

**Interview angle:** Prefer `let` by default; only use `var` when mutation is needed. Shows intent and helps the compiler optimize.

**Resources:** Apple — *The Swift Programming Language: The Basics* (swift.org/documentation); Hacking with Swift — "How to create variables and constants" (hackingwithswift.com).

---

## 1.2 Optionals (the most important basic topic)

**Summary (easy):** An optional is a box that either holds a value or holds nothing (`nil`). It forces you to handle the "no value" case so your app doesn't crash unexpectedly.

**Example:**
```swift
var middleName: String? = nil          // optional, currently empty
if let mid = middleName { print(mid) }  // optional binding
let display = middleName ?? "N/A"       // nil-coalescing
let count = middleName?.count           // optional chaining -> Int?
```

**Key sub-concepts:** optional binding (`if let`, `guard let`), optional chaining (`?.`), nil-coalescing (`??`), force unwrap (`!` — dangerous), implicitly unwrapped optionals (`String!`).

**Interview angle:** Be ready to explain when force-unwrapping is acceptable (almost never in production; OK for `@IBOutlet` / programmer-error invariants) and the crash it causes otherwise.

**Resources:** Apple — *Optionals* chapter; Swift by Sundell — "The power of optionals"; Hacking with Swift — "Unwrapping optionals".

---

## 1.3 Value Types vs Reference Types (struct vs class)

**Summary (easy):** A struct is *copied* when you assign or pass it (value type). A class is *shared* — both variables point to the same object (reference type). Picking the right one prevents whole categories of bugs.

**Example:**
```swift
struct PointS { var x = 0 }
class  PointC { var x = 0 }

var a = PointS(); var b = a; b.x = 5   // a.x == 0 (copy)
let c = PointC(); let d = c; d.x = 5   // c.x == 5 (shared)
```

**Interview angle:** "Why does Apple prefer structs?" → value semantics, thread-safety, no shared mutable state, no retain cycles. Use classes for identity, inheritance, or when you need reference semantics (e.g., a shared cache).

**Resources:** Apple — *Structures and Classes*; WWDC "Building Better Apps with Value Types"; Swift.org evolution discussions.

---

## 1.4 Enums (associated & raw values, indirect)

**Summary (easy):** An enum is a type with a fixed set of cases. Cases can carry extra data (associated values) or map to fixed underlying values (raw values).

**Example:**
```swift
enum Result { case success(data: Data), failure(Error) }   // associated
enum Suit: String { case heart = "♥", spade = "♠" }         // raw value
indirect enum Tree { case leaf(Int), node(Tree, Tree) }     // recursive
```

**Interview angle:** Enums + `switch` give exhaustive, compile-checked state handling. Great answer to "how would you model loading/empty/error states?".

**Resources:** Apple — *Enumerations*; Hacking with Swift — "Enums with associated values".

---

## 1.5 Closures (capture, escaping, trailing, autoclosure)

**Summary (easy):** A closure is a chunk of code you can store and pass around like a value. It "captures" variables from where it was created. `@escaping` means it may run later (after the function returns).

**Example:**
```swift
func load(completion: @escaping (String) -> Void) {
    DispatchQueue.global().async { completion("done") }  // escapes
}
let nums = [3, 1, 2].sorted { $0 < $1 }   // trailing closure + shorthand args
```

**Key sub-concepts:** capture lists `[weak self]`, `@escaping` vs non-escaping, `@autoclosure`, trailing closure syntax, shorthand argument names.

**Interview angle:** Expect "why `[weak self]` in this closure?" — see Phase 3 (memory). Know that non-escaping is the default and why it's cheaper.

**Resources:** Apple — *Closures*; Swift by Sundell — "Capturing objects in Swift closures".

---

## 1.6 Functions (inout, variadic, defaults, labels)

**Summary (easy):** Functions can have default parameter values, take a variable number of args (variadic), and modify a passed value with `inout`. Argument labels make call sites read like sentences.

**Example:**
```swift
func greet(_ name: String, with greeting: String = "Hi") { }
func sum(_ nums: Int...) -> Int { nums.reduce(0, +) }   // variadic
func double(_ x: inout Int) { x *= 2 }                    // inout
```

**Interview angle:** `inout` is not pass-by-reference; it's copy-in/copy-out. Know the difference.

**Resources:** Apple — *Functions*; Hacking with Swift — "Variadic functions".

---

## 1.7 Collections (Array, Set, Dictionary)

**Summary (easy):** Array = ordered list, Set = unordered unique items, Dictionary = key→value pairs. Sets and dictionary keys must be `Hashable`.

**Example:**
```swift
var arr = [1, 2, 2]            // [1, 2, 2]
var set: Set = [1, 2, 2]       // {1, 2}
var dict = ["a": 1, "b": 2]
arr.map { $0 * 2 }; arr.filter { $0 > 1 }; arr.reduce(0, +)
```

**Interview angle:** Know complexity: Set/Dictionary lookup is ~O(1), Array `contains` is O(n). Choosing the right collection is a frequent follow-up.

**Resources:** Apple — *Collection Types*; Swift Algorithms package docs.

---

## 1.8 Control Flow: guard, defer, switch pattern matching

**Summary (easy):** `guard` exits early if a condition fails (keeps the happy path un-indented). `defer` runs code when the scope ends, no matter how. `switch` in Swift can match patterns, ranges, and tuples.

**Example:**
```swift
func process(_ value: Int?) {
    guard let v = value else { return }   // early exit
    defer { print("cleanup") }            // runs last
    switch v {
    case 0: print("zero")
    case 1...9: print("single digit")
    default: print("big")
    }
}
```

**Interview angle:** `guard` for preconditions reads better than nested `if`. `defer` is great for closing files / unlocking locks.

**Resources:** Apple — *Control Flow*; Hacking with Swift — "guard / defer".

---

## 1.9 Error Handling (throws, try, Result)

**Summary (easy):** A function can `throw` an error; the caller handles it with `do/try/catch`. `Result<Success, Failure>` packages success-or-failure into a value (useful for async callbacks).

**Example:**
```swift
enum NetError: Error { case offline }
func fetch() throws -> Data { throw NetError.offline }

do { let d = try fetch() }
catch NetError.offline { print("no internet") }
catch { print("other: \(error)") }

let r: Result<Int, Error> = .success(5)
```

**Key sub-concepts:** `try`, `try?`, `try!`, `throws`, `rethrows`, typed errors, `Result`.

**Interview angle:** Know the difference between `try?` (turns error into `nil`) and `try!` (crash on error).

**Resources:** Apple — *Error Handling*; Swift by Sundell — "Result type".

---

## 1.10 Tuples & Pattern Matching

**Summary (easy):** A tuple groups several values into one without making a whole new type. Handy for returning multiple values or quick local grouping.

**Example:**
```swift
func minMax(_ a: [Int]) -> (min: Int, max: Int) { (a.min()!, a.max()!) }
let result = minMax([3, 1, 8])
print(result.max)   // 8
```

**Interview angle:** Tuples are great for tiny, local groupings; prefer a named struct once the group has meaning across your code.

**Resources:** Apple — *Tuples* (in The Basics); Hacking with Swift — "Tuples".
