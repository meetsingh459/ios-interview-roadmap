[⬅ Back to Roadmap](../README.md)

# Phase 4 — Concurrency

The single biggest differentiator for senior iOS interviews. Know both the classic stack (GCD/Operations) and modern Swift Concurrency (async/await, actors), and *why* the new model exists.

---

## 4.1 GCD — Queues, sync/async, QoS

**Summary (easy):** Grand Central Dispatch runs work on queues. Serial queues run one task at a time; concurrent queues run many. `async` schedules and returns immediately; `sync` blocks until done. QoS tells the system how important the work is.

**Example:**
```swift
DispatchQueue.global(qos: .userInitiated).async {
    let data = heavyWork()
    DispatchQueue.main.async { updateUI(with: data) }
}
```

**Interview angle:** Never call `sync` on the current queue (deadlock). UI must update on the main queue. Know the QoS classes: userInteractive, userInitiated, default, utility, background.

**Resources:** Apple — *Dispatch*; Kodeco — "GCD Tutorial"; raywenderlich/Kodeco threading book.

---

## 4.2 DispatchGroup, Semaphore, WorkItem, Barrier

**Summary (easy):** `DispatchGroup` waits for several async tasks to all finish. `DispatchSemaphore` limits concurrent access to N. `DispatchWorkItem` is a cancelable unit of work. A `.barrier` flag makes a write block all reads on a concurrent queue (reader-writer lock).

**Example:**
```swift
let group = DispatchGroup()
for url in urls {
    group.enter()
    download(url) { group.leave() }
}
group.notify(queue: .main) { print("all done") }
```

**Interview angle:** Classic question: build a thread-safe property using a concurrent queue + `.barrier` for writes and plain `.sync` for reads.

**Resources:** Apple — *DispatchGroup / DispatchSemaphore*; SwiftLee — "Concurrent vs serial".

---

## 4.3 Operations & OperationQueue

**Summary (easy):** `Operation` is an object-oriented unit of work supporting dependencies, cancellation, priorities, and max-concurrent limits — a higher-level abstraction over GCD.

**Example:**
```swift
let queue = OperationQueue()
queue.maxConcurrentOperationCount = 2
let download = BlockOperation { /* fetch */ }
let resize = BlockOperation { /* process */ }
resize.addDependency(download)   // resize waits for download
queue.addOperations([download, resize], waitUntilFinished: false)
```

**Interview angle:** "GCD vs Operations?" → Operations give cancellation, dependencies, observable state; GCD is lighter for fire-and-forget.

**Resources:** Apple — *Operation / OperationQueue*; objc.io concurrency articles.

---

## 4.4 Thread Safety: Race Conditions, Deadlocks, Locks

**Summary (easy):** A race condition is two threads touching shared mutable state at the same time with unpredictable results. A deadlock is two threads each waiting on the other forever. Locks (`NSLock`, `os_unfair_lock`) or a serial queue prevent races.

**Example:**
```swift
final class Counter {
    private var value = 0
    private let lock = NSLock()
    func increment() { lock.lock(); value += 1; lock.unlock() }
}
```

**Interview angle:** Be able to *identify* a race and offer 2–3 fixes (serial queue, lock, actor). Mention Thread Sanitizer (TSan) for detecting them.

**Resources:** Apple — *Thread Safety*; WWDC "Thread Sanitizer and Static Analysis".

---

## 4.5 async / await

**Summary (easy):** `async`/`await` lets you write asynchronous code that *reads* top-to-bottom like synchronous code. `await` is a suspension point — the thread is freed to do other work while waiting.

**Example:**
```swift
func loadProfile() async throws -> Profile {
    let data = try await URLSession.shared.data(from: url).0
    return try JSONDecoder().decode(Profile.self, from: data)
}
```

**Interview angle:** Explain "suspension point" and how it differs from blocking a thread. Know how to call async code from a synchronous context with `Task { }`.

**Resources:** Apple — *Concurrency* (Swift book); WWDC21 "Meet async/await"; Donny Wals — async/await series.

---

## 4.6 Tasks, Task Groups & Structured Concurrency

**Summary (easy):** A `Task` runs async work. Structured concurrency means child tasks are tied to a parent scope — if the parent is cancelled or finishes, children are handled automatically. `withTaskGroup` runs many child tasks in parallel and collects results.

**Example:**
```swift
await withTaskGroup(of: Data.self) { group in
    for url in urls { group.addTask { try? await fetch(url) } }
    for await data in group { handle(data) }
}
```

**Interview angle:** Know `async let` for a few parallel calls vs task groups for a dynamic number. Know cooperative cancellation (`Task.checkCancellation()`).

**Resources:** WWDC21 "Explore structured concurrency in Swift"; Apple docs.

---

## 4.7 Actors, @MainActor & Sendable

**Summary (easy):** An `actor` protects its mutable state so only one task touches it at a time (no manual locks). `@MainActor` pins code/properties to the main thread (great for UI). `Sendable` marks types safe to pass across concurrency boundaries.

**Example:**
```swift
actor BankAccount {
    private var balance = 0
    func deposit(_ x: Int) { balance += x }   // automatically serialized
}
@MainActor func updateLabel() { label.text = "done" }
```

**Interview angle:** Explain "actor reentrancy" (state can change across an `await` inside an actor method) — a senior-level gotcha. Know why `@MainActor` replaces `DispatchQueue.main.async`.

**Resources:** WWDC21 "Protect mutable state with actors"; Apple — *Actors*; Swift 6 data-race safety docs.

---

## 4.8 AsyncSequence, AsyncStream & Continuations

**Summary (easy):** `AsyncSequence` is a sequence you iterate with `for await`. `AsyncStream` lets you build one from callbacks/events. Continuations (`withCheckedContinuation`) bridge old completion-handler APIs into async/await.

**Example:**
```swift
func legacyToAsync() async -> Data {
    await withCheckedContinuation { cont in
        oldAPI { data in cont.resume(returning: data) }
    }
}
```

**Interview angle:** Bridging legacy callbacks is a very common real-world + interview task. Always resume a continuation exactly once.

**Resources:** WWDC21 "Meet AsyncSequence"; Apple — *Continuations*; Donny Wals articles.
