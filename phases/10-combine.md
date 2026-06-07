[⬅ Back to Roadmap](../README.md)

# Phase 10 — Combine & Reactive Programming

Many existing codebases use Combine. Even as async/await grows, interviewers ask about publishers, operators, and memory management.

---

## 10.1 Publishers, Subscribers & the Combine Model

**Summary (easy):** Combine is a framework for handling values over time. A *publisher* emits values; a *subscriber* receives them; *operators* transform the stream in between. It's pull-based with backpressure.

**Example:**
```swift
let cancellable = [1, 2, 3].publisher
    .map { $0 * 2 }
    .sink { print($0) }   // 2 4 6
```

**Interview angle:** Explain the three publisher events: value(s), completion (finished/failure). Know that a subscription is kept alive by its `Cancellable`.

**Resources:** Apple — *Combine*; Kodeco "Combine: Asynchronous Programming with Swift" book.

---

## 10.2 @Published & ObservableObject

**Summary (easy):** `@Published` turns a property into a publisher; combined with `ObservableObject`, SwiftUI views re-render when it changes. This was the standard view-model pattern before the Observation framework.

**Example:**
```swift
class SearchVM: ObservableObject {
    @Published var query = ""
    @Published var results: [String] = []
}
```

**Interview angle:** Access the publisher with the `$` projected value: `$query`. Compare to the newer `@Observable`.

**Resources:** Apple docs; Hacking with Swift Combine articles.

---

## 10.3 Subjects (PassthroughSubject, CurrentValueSubject)

**Summary (easy):** Subjects are publishers you can imperatively send values into. `PassthroughSubject` has no initial value; `CurrentValueSubject` holds and replays its latest value to new subscribers.

**Example:**
```swift
let events = PassthroughSubject<String, Never>()
let state = CurrentValueSubject<Int, Never>(0)
events.send("tapped"); state.send(1)
```

**Interview angle:** Use `CurrentValueSubject` when subscribers need the current state immediately (like a store).

**Resources:** Apple — *Subjects*; Donny Wals Combine series.

---

## 10.4 Operators (map, flatMap, combineLatest, merge, zip, debounce)

**Summary (easy):** Operators reshape streams: `map` transforms, `flatMap` swaps in a new publisher, `combineLatest`/`zip`/`merge` join streams, `debounce` waits for a pause (great for search-as-you-type), `removeDuplicates` filters repeats.

**Example:**
```swift
$query
    .debounce(for: .milliseconds(300), scheduler: RunLoop.main)
    .removeDuplicates()
    .sink { search($0) }
    .store(in: &cancellables)
```

**Interview angle:** The debounced search pipeline is a frequent live exercise. Know `combineLatest` vs `zip` (latest-of-each vs paired).

**Resources:** Kodeco Combine book (operators); RxMarbles-style diagrams.

---

## 10.5 Cancellables & Memory Management in Combine

**Summary (easy):** A subscription returns an `AnyCancellable`. Store it (usually in a `Set<AnyCancellable>`) to keep it alive; when it's deallocated, the subscription cancels. Forgetting to store it means the stream dies immediately.

**Example:**
```swift
private var cancellables = Set<AnyCancellable>()
publisher.sink { _ in }.store(in: &cancellables)
```

**Interview angle:** Watch for retain cycles in `sink` closures capturing `self` strongly — use `[weak self]`.

**Resources:** Apple — *AnyCancellable*; SwiftLee Combine memory articles.

---

## 10.6 Schedulers, Error Handling & Combine vs async/await

**Summary (easy):** `receive(on:)`/`subscribe(on:)` control which thread work runs on (move UI updates to `.main`). Errors flow through the completion; `catch`/`retry`/`replaceError` handle them. async/await is simpler for one-shot calls; Combine shines for ongoing streams.

**Example:**
```swift
publisher
    .receive(on: DispatchQueue.main)
    .retry(2)
    .catch { _ in Just(fallback) }
    .sink { update($0) }.store(in: &cancellables)
```

**Interview angle:** Be able to say *when* you'd pick Combine (event streams, multiple subscribers, complex pipelines) vs async/await (linear async tasks). Know `.values` bridges a publisher into an `AsyncSequence`.

**Resources:** Apple — *Combine schedulers*; WWDC "Combine in Practice".
