# 📱 iOS Interview Preparation Roadmap (0 → 100)

A step-by-step roadmap to prepare for **iOS-specific interview rounds** at product-based companies (Google, Amazon, Netflix, Apple, etc.) targeting **SDE-2 / SDE-3 (Senior)** levels.

Each topic links to a **detail page** with a beginner-friendly summary, a short code example, an "interview angle" note, and external resources to go deeper.

> 💡 **Tip:** Tap a topic to open its detail page. On desktop, `Cmd`/`Ctrl + click` (or middle-click) opens it in a **new tab**.

> 🚫 **Out of scope (on purpose):** **System Design** and **Data Structures & Algorithms (DSA)** are separate interview tracks — prepare those separately. This roadmap is strictly **iOS domain knowledge**.

---

## ✅ How to use this roadmap

1. Go **top to bottom** — topics are ordered basic → advanced and build on each other.
2. Read the detail page, then follow at least one external resource for depth.
3. Tick the checkbox once you can **explain it out loud + write the example from memory**.
4. For each topic, practice answering: *"What is it? When would you use it? What's the common trap?"*

**Suggested pace:** ~2 phases/week for a solid 7-week plan; compress to ~3 weeks if revising.

---

## 📊 Track your progress

You have **two ways** to track how far you've come:

**Option A — Interactive dashboard (recommended).** Open [`tracker.html`](tracker.html) for a visual board with an overall completion ring, per-phase progress bars, To-do / Done filters, and encouragement as you climb. Your checkmarks are saved automatically in your browser (`localStorage`) — no account, no backend.
- **Locally:** download the repo and open `tracker.html` in any browser.
- **Online:** enable **GitHub Pages** (repo *Settings → Pages → deploy from `main` / root*), then visit `https://<your-username>.github.io/<repo-name>/tracker.html`.
- *(Note: progress is stored per-browser, so it won't sync between your laptop and phone.)*

**Option B — Checkboxes in this README.** The roadmap below is a GitHub task list. Tick a box by editing the file (pencil icon on GitHub, or locally) — GitHub shows a live `x / total` count at the top of the list, and the state is committed to your repo so it syncs everywhere.

> Use **A** for a slick daily dashboard, **B** if you want your progress version-controlled in the repo itself. Many people use both.

---

## 🗺️ The Roadmap

### Phase 1 — [Swift Language Fundamentals](phases/01-swift-fundamentals.md)
- [ ] [1.1 Variables, Constants & Type Inference](phases/01-swift-fundamentals.md#11-variables-constants--type-inference)
- [ ] [1.2 Optionals (binding, chaining, nil-coalescing, force unwrap)](phases/01-swift-fundamentals.md#12-optionals-the-most-important-basic-topic)
- [ ] [1.3 Value Types vs Reference Types (struct vs class)](phases/01-swift-fundamentals.md#13-value-types-vs-reference-types-struct-vs-class)
- [ ] [1.4 Enums (associated & raw values, indirect)](phases/01-swift-fundamentals.md#14-enums-associated--raw-values-indirect)
- [ ] [1.5 Closures (capture, escaping, trailing, autoclosure)](phases/01-swift-fundamentals.md#15-closures-capture-escaping-trailing-autoclosure)
- [ ] [1.6 Functions (inout, variadic, defaults, labels)](phases/01-swift-fundamentals.md#16-functions-inout-variadic-defaults-labels)
- [ ] [1.7 Collections (Array, Set, Dictionary)](phases/01-swift-fundamentals.md#17-collections-array-set-dictionary)
- [ ] [1.8 Control Flow: guard, defer, switch pattern matching](phases/01-swift-fundamentals.md#18-control-flow-guard-defer-switch-pattern-matching)
- [ ] [1.9 Error Handling (throws, try, Result)](phases/01-swift-fundamentals.md#19-error-handling-throws-try-result)
- [ ] [1.10 Tuples & Pattern Matching](phases/01-swift-fundamentals.md#110-tuples--pattern-matching)

### Phase 2 — [Intermediate Swift & The Type System](phases/02-swift-type-system.md)
- [ ] [2.1 Protocols & Protocol-Oriented Programming](phases/02-swift-type-system.md#21-protocols--protocol-oriented-programming-pop)
- [ ] [2.2 Protocol Extensions & Default Implementations](phases/02-swift-type-system.md#22-protocol-extensions--default-implementations)
- [ ] [2.3 Generics (constraints, where clauses)](phases/02-swift-type-system.md#23-generics-constraints-where-clauses)
- [ ] [2.4 Associated Types](phases/02-swift-type-system.md#24-associated-types)
- [ ] [2.5 Opaque Types (`some`) vs Existentials (`any`)](phases/02-swift-type-system.md#25-opaque-types-some-vs-existentials-any)
- [ ] [2.6 Property Wrappers](phases/02-swift-type-system.md#26-property-wrappers)
- [ ] [2.7 Codable (Encoding & Decoding)](phases/02-swift-type-system.md#27-codable-encoding--decoding)
- [ ] [2.8 Equatable, Hashable, Comparable, Identifiable](phases/02-swift-type-system.md#28-equatable-hashable-comparable-identifiable)
- [ ] [2.9 Type Casting & `Any`/`AnyObject`](phases/02-swift-type-system.md#29-type-casting--anyanyobject)
- [ ] [2.10 Extensions, Computed Properties & Observers](phases/02-swift-type-system.md#210-extensions-computed-properties--observers)

### Phase 3 — [Memory Management (ARC)](phases/03-memory-management.md)
- [ ] [3.1 ARC — Automatic Reference Counting](phases/03-memory-management.md#31-arc--automatic-reference-counting)
- [ ] [3.2 strong / weak / unowned](phases/03-memory-management.md#32-strong--weak--unowned)
- [ ] [3.3 Retain Cycles (memory leaks)](phases/03-memory-management.md#33-retain-cycles-memory-leaks)
- [ ] [3.4 Capture Lists in Closures ([weak self])](phases/03-memory-management.md#34-capture-lists-in-closures-weak-self--unowned-self)
- [ ] [3.5 Value Semantics & Copy-on-Write (COW)](phases/03-memory-management.md#35-value-semantics--copy-on-write-cow)
- [ ] [3.6 Stack vs Heap, autoreleasepool, Memory Graph Debugger](phases/03-memory-management.md#36-stack-vs-heap-autoreleasepool-memory-graph-debugger)

### Phase 4 — [Concurrency](phases/04-concurrency.md)
- [ ] [4.1 GCD — Queues, sync/async, QoS](phases/04-concurrency.md#41-gcd--queues-syncasync-qos)
- [ ] [4.2 DispatchGroup, Semaphore, WorkItem, Barrier](phases/04-concurrency.md#42-dispatchgroup-semaphore-workitem-barrier)
- [ ] [4.3 Operations & OperationQueue](phases/04-concurrency.md#43-operations--operationqueue)
- [ ] [4.4 Thread Safety: Race Conditions, Deadlocks, Locks](phases/04-concurrency.md#44-thread-safety-race-conditions-deadlocks-locks)
- [ ] [4.5 async / await](phases/04-concurrency.md#45-async--await)
- [ ] [4.6 Tasks, Task Groups & Structured Concurrency](phases/04-concurrency.md#46-tasks-task-groups--structured-concurrency)
- [ ] [4.7 Actors, @MainActor & Sendable](phases/04-concurrency.md#47-actors-mainactor--sendable)
- [ ] [4.8 AsyncSequence, AsyncStream & Continuations](phases/04-concurrency.md#48-asyncsequence-asyncstream--continuations)

### Phase 5 — [UIKit Fundamentals](phases/05-uikit.md)
- [ ] [5.1 UIViewController Lifecycle](phases/05-uikit.md#51-uiviewcontroller-lifecycle)
- [ ] [5.2 UIView: frame vs bounds, layer, coordinate systems](phases/05-uikit.md#52-uiview-frame-vs-bounds-layer-coordinate-systems)
- [ ] [5.3 Auto Layout (constraints, priorities, hugging/compression)](phases/05-uikit.md#53-auto-layout-constraints-priorities-huggingcompression)
- [ ] [5.4 Stack Views](phases/05-uikit.md#54-stack-views)
- [ ] [5.5 UITableView (reuse, delegate/datasource, diffable)](phases/05-uikit.md#55-uitableview-reuse-delegatedatasource-diffable)
- [ ] [5.6 UICollectionView & Compositional Layout](phases/05-uikit.md#56-uicollectionview--compositional-layout)
- [ ] [5.7 Responder Chain & Gesture Recognizers](phases/05-uikit.md#57-responder-chain--gesture-recognizers)
- [ ] [5.8 Navigation, Presentation & Programmatic vs Storyboard UI](phases/05-uikit.md#58-navigation-presentation--programmatic-vs-storyboard-ui)
- [ ] [5.9 App & Scene Lifecycle, Run Loop](phases/05-uikit.md#59-app--scene-lifecycle-run-loop)

### Phase 6 — [SwiftUI](phases/06-swiftui.md)
- [ ] [6.1 Declarative UI, the View protocol & body](phases/06-swiftui.md#61-declarative-ui-the-view-protocol--body)
- [ ] [6.2 @State & @Binding](phases/06-swiftui.md#62-state--binding)
- [ ] [6.3 @StateObject vs @ObservedObject vs @EnvironmentObject](phases/06-swiftui.md#63-stateobject-vs-observedobject-vs-environmentobject)
- [ ] [6.4 The Observation framework (@Observable, iOS 17+)](phases/06-swiftui.md#64-the-observation-framework-observable-ios-17)
- [ ] [6.5 @Environment & Dependency Injection in SwiftUI](phases/06-swiftui.md#65-environment--dependency-injection-in-swiftui)
- [ ] [6.6 Layout System (stacks, GeometryReader, Layout protocol)](phases/06-swiftui.md#66-layout-system-stacks-geometryreader-alignment-layout-protocol)
- [ ] [6.7 Lists, ForEach & Identifiable](phases/06-swiftui.md#67-lists-foreach--identifiable)
- [ ] [6.8 Navigation (NavigationStack & NavigationPath)](phases/06-swiftui.md#68-navigation-navigationstack--navigationpath)
- [ ] [6.9 Modifiers, Animations & Transitions](phases/06-swiftui.md#69-modifiers-animations--transitions)
- [ ] [6.10 UIKit ↔ SwiftUI Interoperability](phases/06-swiftui.md#610-uikit--swiftui-interoperability)

### Phase 7 — [Architecture & Design Patterns](phases/07-architecture.md)
- [ ] [7.1 MVC, MVP, MVVM, VIPER, Clean, TCA](phases/07-architecture.md#71-mvc-mvp-mvvm-viper-clean-tca)
- [ ] [7.2 Delegation Pattern](phases/07-architecture.md#72-delegation-pattern)
- [ ] [7.3 Observer Pattern: NotificationCenter & KVO](phases/07-architecture.md#73-observer-pattern-notificationcenter--kvo)
- [ ] [7.4 Singleton (and its pitfalls)](phases/07-architecture.md#74-singleton-and-its-pitfalls)
- [ ] [7.5 Dependency Injection (DI)](phases/07-architecture.md#75-dependency-injection-di)
- [ ] [7.6 Coordinator Pattern](phases/07-architecture.md#76-coordinator-pattern)
- [ ] [7.7 Factory, Builder, Adapter, Strategy, Repository](phases/07-architecture.md#77-factory-builder-adapter-strategy-repository)
- [ ] [7.8 SOLID Principles & Composition over Inheritance](phases/07-architecture.md#78-solid-principles--composition-over-inheritance)

### Phase 8 — [Networking](phases/08-networking.md)
- [ ] [8.1 URLSession Basics (data / download / upload tasks)](phases/08-networking.md#81-urlsession-basics-data--download--upload-tasks)
- [ ] [8.2 REST, HTTP Methods & Status Codes](phases/08-networking.md#82-rest-http-methods--status-codes)
- [ ] [8.3 JSON Parsing with Codable + Error Handling](phases/08-networking.md#83-json-parsing-with-codable--error-handling)
- [ ] [8.4 Authentication (tokens, OAuth, JWT, refresh)](phases/08-networking.md#84-authentication-tokens-oauth-jwt-refresh)
- [ ] [8.5 Caching (URLCache) & Conditional Requests](phases/08-networking.md#85-caching-urlcache--conditional-requests)
- [ ] [8.6 WebSockets & Streaming](phases/08-networking.md#86-websockets--streaming)
- [ ] [8.7 Networking Libraries & Security (Alamofire, pinning)](phases/08-networking.md#87-networking-libraries--security-alamofire-certificate-pinning)

### Phase 9 — [Persistence & Data Storage](phases/09-persistence.md)
- [ ] [9.1 UserDefaults](phases/09-persistence.md#91-userdefaults)
- [ ] [9.2 Keychain](phases/09-persistence.md#92-keychain)
- [ ] [9.3 File System (FileManager)](phases/09-persistence.md#93-file-system-filemanager)
- [ ] [9.4 Core Data (stack, contexts, fetch, concurrency)](phases/09-persistence.md#94-core-data-stack-contexts-fetch-concurrency)
- [ ] [9.5 SwiftData (iOS 17+)](phases/09-persistence.md#95-swiftdata-ios-17)
- [ ] [9.6 SQLite / GRDB / Realm (awareness)](phases/09-persistence.md#96-sqlite--grdb--realm-awareness)
- [ ] [9.7 Migration Strategies](phases/09-persistence.md#97-migration-strategies)

### Phase 10 — [Combine & Reactive Programming](phases/10-combine.md)
- [ ] [10.1 Publishers, Subscribers & the Combine Model](phases/10-combine.md#101-publishers-subscribers--the-combine-model)
- [ ] [10.2 @Published & ObservableObject](phases/10-combine.md#102-published--observableobject)
- [ ] [10.3 Subjects (PassthroughSubject, CurrentValueSubject)](phases/10-combine.md#103-subjects-passthroughsubject-currentvaluesubject)
- [ ] [10.4 Operators (map, flatMap, combineLatest, debounce…)](phases/10-combine.md#104-operators-map-flatmap-combinelatest-merge-zip-debounce)
- [ ] [10.5 Cancellables & Memory Management in Combine](phases/10-combine.md#105-cancellables--memory-management-in-combine)
- [ ] [10.6 Schedulers, Error Handling & Combine vs async/await](phases/10-combine.md#106-schedulers-error-handling--combine-vs-asyncawait)

### Phase 11 — [Testing](phases/11-testing.md)
- [ ] [11.1 Unit Testing with XCTest](phases/11-testing.md#111-unit-testing-with-xctest)
- [ ] [11.2 Test Doubles: Mocks, Stubs, Spies, Fakes](phases/11-testing.md#112-test-doubles-mocks-stubs-spies-fakes)
- [ ] [11.3 Designing for Testability (DI, protocols, pure functions)](phases/11-testing.md#113-designing-for-testability-di-protocols-pure-functions)
- [ ] [11.4 Testing Async Code (expectations & async tests)](phases/11-testing.md#114-testing-async-code-expectations--async-tests)
- [ ] [11.5 UI Testing (XCUITest)](phases/11-testing.md#115-ui-testing-xcuitest)
- [ ] [11.6 Snapshot Testing, Coverage & Swift Testing](phases/11-testing.md#116-snapshot-testing-coverage--swift-testing)

### Phase 12 — [Performance & Optimization](phases/12-performance.md)
- [ ] [12.1 Instruments (Time Profiler, Allocations, Leaks, Core Animation)](phases/12-performance.md#121-instruments-time-profiler-allocations-leaks-core-animation)
- [ ] [12.2 Main Thread Hygiene & Avoiding Blocking](phases/12-performance.md#122-main-thread-hygiene--avoiding-blocking)
- [ ] [12.3 List/Collection Performance (reuse, prefetching, sizing)](phases/12-performance.md#123-listcollection-performance-reuse-prefetching-sizing)
- [ ] [12.4 Image & Asset Optimization](phases/12-performance.md#124-image--asset-optimization)
- [ ] [12.5 App Launch Time Optimization](phases/12-performance.md#125-app-launch-time-optimization)
- [ ] [12.6 Build Time & Compiler Optimization](phases/12-performance.md#126-build-time--compiler-optimization)

### Phase 13 — [App Lifecycle, Tooling & Release](phases/13-lifecycle-tooling-release.md)
- [ ] [13.1 App States & Background Execution](phases/13-lifecycle-tooling-release.md#131-app-states--background-execution)
- [ ] [13.2 Push & Local Notifications](phases/13-lifecycle-tooling-release.md#132-push--local-notifications)
- [ ] [13.3 Deep Linking & Universal Links](phases/13-lifecycle-tooling-release.md#133-deep-linking--universal-links)
- [ ] [13.4 Code Signing, Provisioning & Certificates](phases/13-lifecycle-tooling-release.md#134-code-signing-provisioning--certificates)
- [ ] [13.5 CI/CD (Fastlane, Xcode Cloud)](phases/13-lifecycle-tooling-release.md#135-cicd-fastlane-xcode-cloud)
- [ ] [13.6 App Store, TestFlight, Crash Reporting & Feature Flags](phases/13-lifecycle-tooling-release.md#136-app-store-testflight-crash-reporting--feature-flags)
- [ ] [13.7 App Thinning, Bitcode & App Size](phases/13-lifecycle-tooling-release.md#137-app-thinning-bitcode--app-size)

### Phase 14 — [Advanced & Niche Topics](phases/14-advanced.md)
- [ ] [14.1 Method Dispatch (static, dynamic, witness table, vtable)](phases/14-advanced.md#141-method-dispatch-static-dynamic-witness-table-vtable)
- [ ] [14.2 Objective-C Interop (@objc, dynamic, bridging)](phases/14-advanced.md#142-objective-c-interop-objc-dynamic-bridging)
- [ ] [14.3 Method Swizzling & the Runtime](phases/14-advanced.md#143-method-swizzling--the-runtime)
- [ ] [14.4 Result Builders (@resultBuilder)](phases/14-advanced.md#144-result-builders-resultbuilder)
- [ ] [14.5 Macros (Swift 5.9+)](phases/14-advanced.md#145-macros-swift-59)
- [ ] [14.6 Modules, Frameworks & Swift Package Manager](phases/14-advanced.md#146-modules-frameworks--swift-package-manager)
- [ ] [14.7 Accessibility (VoiceOver, Dynamic Type)](phases/14-advanced.md#147-accessibility-voiceover-dynamic-type)
- [ ] [14.8 Localization, Security & Reflection](phases/14-advanced.md#148-localization-security--reflection)

---

## 🎯 The 5 Common iOS Interview Rounds (what to expect)

| Round | What they test | Where this roadmap helps |
|------|----------------|--------------------------|
| iOS Domain / Fundamentals | Swift, memory, concurrency, UIKit/SwiftUI depth | Phases 1–6, 14 |
| Practical / Live Coding (iOS) | Build a small feature, fix a leak, write a view model | Phases 1–11 |
| Architecture / Code Design | Structuring a feature, trade-offs, testability | Phases 7, 11 |
| Debugging / Performance | Diagnose jank, leaks, crashes | Phases 3, 4, 12 |
| Behavioral / Past Projects | Ownership, impact, collaboration | *(prepare separately)* |

> System Design and DSA rounds also exist for SDE-2/3 — prepare those in your separate tracks.

---

## 📚 Top General Resources (evergreen)

- **Apple Developer Documentation** — developer.apple.com/documentation (source of truth)
- **The Swift Programming Language** — docs.swift.org (free, official Swift book)
- **Hacking with Swift** (Paul Hudson) — hackingwithswift.com (free articles + "100 Days")
- **Kodeco** (ex-raywenderlich) — kodeco.com (tutorials + books)
- **Swift by Sundell** (John Sundell) — swiftbysundell.com (deep articles + podcast)
- **WWDC Sessions** — developer.apple.com/videos (best for modern APIs)
- **objc.io** — books "Advanced Swift", "Thinking in SwiftUI", "App Architecture"
- **Point-Free** — pointfree.co (functional Swift, testing, architecture)
- **Blogs:** SwiftLee (avanderlee.com), Donny Wals (donnywals.com), Use Your Loaf (useyourloaf.com)
- **Interview question banks:** search "iOS interview questions" repos on GitHub for rapid-fire drills

---

## 🤝 Contributing

Found a mistake or want to add a topic? Open an issue or a pull request. Keep detail pages in the same format: **Summary (easy) → Example → Interview angle → Resources**.

## 📄 License

Free to use, share, and adapt. Attribution appreciated.

---

⭐ If this helped you prep, consider starring the repo so others can find it. Good luck with your interviews!
