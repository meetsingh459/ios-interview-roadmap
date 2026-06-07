[⬅ Back to Roadmap](../README.md)

# Phase 11 — Testing

SDE-2/3 candidates are expected to write testable code and explain a testing strategy. This is often where strong engineers stand out.

---

## 11.1 Unit Testing with XCTest

**Summary (easy):** A unit test verifies one small piece of logic in isolation. XCTest provides `XCTestCase` subclasses, `setUp`/`tearDown`, and assertions (`XCTAssertEqual`, etc.). Follow Arrange–Act–Assert.

**Example:**
```swift
final class CalculatorTests: XCTestCase {
    func testAddition() {
        let sut = Calculator()           // Arrange
        let result = sut.add(2, 3)       // Act
        XCTAssertEqual(result, 5)        // Assert
    }
}
```

**Interview angle:** Know "SUT" (system under test), the FIRST principles (Fast, Isolated, Repeatable, Self-validating, Timely), and what makes a *good* unit test.

**Resources:** Apple — *XCTest*; Kodeco — "iOS Unit Testing and UI Testing Tutorial".

---

## 11.2 Test Doubles: Mocks, Stubs, Spies, Fakes

**Summary (easy):** Fake collaborators let you isolate the SUT. A *stub* returns canned values; a *mock* verifies interactions happened; a *spy* records calls; a *fake* is a lightweight working implementation (e.g., in-memory DB).

**Example:**
```swift
final class APIStub: APIClient {
    var stubbedUsers: [User] = []
    func fetchUsers() async -> [User] { stubbedUsers }
}
```

**Interview angle:** Be able to define each term precisely and explain why DI (Phase 7) is what makes them possible.

**Resources:** Martin Fowler — "Mocks Aren't Stubs"; Swift by Sundell — "Mocking in Swift".

---

## 11.3 Designing for Testability (DI, protocols, pure functions)

**Summary (easy):** Code is testable when dependencies are injectable and side effects are isolated. Hide collaborators behind protocols, push logic into pure functions, and avoid hidden global state/singletons.

**Example:**
```swift
// Inject a clock so time-based logic is deterministic
protocol Clock { func now() -> Date }
struct FixedClock: Clock { let date: Date; func now() -> Date { date } }
```

**Interview angle:** Expect "this class is hard to test — how would you refactor it?" Answer with DI, protocol abstraction, and separating I/O from logic.

**Resources:** Point-Free — testability episodes; "Working Effectively with Legacy Code" (Feathers).

---

## 11.4 Testing Async Code (expectations & async tests)

**Summary (easy):** For callback-based async, use `XCTestExpectation` and `wait(for:timeout:)`. For modern code, mark the test `async` and `await` directly.

**Example:**
```swift
func testLoad() async throws {
    let vm = ProfileVM(api: APIStub())
    await vm.load()
    XCTAssertNotNil(vm.user)
}
```

**Interview angle:** Know both styles. Mention pitfalls: flaky tests from real timers/network → inject fakes and a controllable scheduler.

**Resources:** Apple — *Asynchronous tests and expectations*; WWDC async testing sessions.

---

## 11.5 UI Testing (XCUITest)

**Summary (easy):** XCUITest drives the app like a user — taps, types, asserts elements exist. It uses accessibility identifiers to find elements. Slower and more brittle than unit tests, so use sparingly for critical flows.

**Example:**
```swift
let app = XCUIApplication(); app.launch()
app.buttons["loginButton"].tap()
XCTAssertTrue(app.staticTexts["Welcome"].exists)
```

**Interview angle:** Know the test pyramid: many unit, fewer integration, fewest UI tests. Stable selectors via accessibility IDs.

**Resources:** Apple — *XCUITest*; Kodeco UI testing tutorial.

---

## 11.6 Snapshot Testing, Coverage & Swift Testing

**Summary (easy):** Snapshot tests render a view and compare it to a reference image to catch UI regressions. Code coverage shows which lines tests exercise. *Swift Testing* (the new framework) uses `@Test` and `#expect` macros — cleaner than XCTest.

**Example (Swift Testing):**
```swift
import Testing
@Test func addsNumbers() {
    #expect(Calculator().add(2, 3) == 5)
}
```

**Interview angle:** Know that high coverage ≠ good tests (you can cover lines without asserting behavior). Be aware Swift Testing is the modern direction alongside XCTest.

**Resources:** WWDC24 "Meet Swift Testing"; pointfreeco/swift-snapshot-testing; Apple coverage docs.
