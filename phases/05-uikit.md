[⬅ Back to Roadmap](../README.md)

# Phase 5 — UIKit Fundamentals

Even SwiftUI-heavy teams expect UIKit fluency at SDE-2/3. Lifecycle, Auto Layout, and table/collection views are perennial questions.

---

## 5.1 UIViewController Lifecycle

**Summary (easy):** A view controller goes through ordered callbacks as it appears and disappears. Knowing the order tells you where to put setup, layout, and teardown code.

**Example (order):**
```
init → loadView → viewDidLoad (once) →
viewWillAppear → viewWillLayoutSubviews → viewDidLayoutSubviews →
viewDidAppear → ... → viewWillDisappear → viewDidDisappear
```

**Interview angle:** `viewDidLoad` runs once (do one-time setup); `viewWillAppear` can run many times (refresh data). Frame-dependent work belongs in `viewDidLayoutSubviews`, not `viewDidLoad`.

**Resources:** Apple — *UIViewController*; Use Your Loaf — "View controller lifecycle".

---

## 5.2 UIView: frame vs bounds, layer, coordinate systems

**Summary (easy):** `frame` is the view's position+size in its *parent's* coordinates. `bounds` is its own internal coordinate space (origin usually 0,0). Every view is backed by a `CALayer` that does the actual drawing.

**Example:**
```swift
view.frame   // e.g. (x:20, y:50, w:100, h:40) in superview
view.bounds  // e.g. (x:0,  y:0,  w:100, h:40)
view.layer.cornerRadius = 8
```

**Interview angle:** "What changes when you rotate a view 45°?" → `frame` grows (bounding box) but `bounds` stays the same. Great differentiator question.

**Resources:** Apple — *UIView*; "Frame vs Bounds" classic articles.

---

## 5.3 Auto Layout (constraints, priorities, hugging/compression)

**Summary (easy):** Auto Layout positions views with relationships (constraints) instead of fixed coordinates, so layouts adapt to screen sizes. Content hugging resists growing; compression resistance resists shrinking.

**Example:**
```swift
label.translatesAutoresizingMaskIntoConstraints = false
NSLayoutConstraint.activate([
    label.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 16),
    label.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor)
])
```

**Interview angle:** Be ready to debug "Unable to simultaneously satisfy constraints," explain priorities (1–1000), and intrinsic content size.

**Resources:** Apple — *Auto Layout Guide*; Kodeco — "Auto Layout Tutorial".

---

## 5.4 Stack Views

**Summary (easy):** `UIStackView` arranges subviews in a row or column and manages their constraints for you (axis, spacing, distribution, alignment) — far less boilerplate.

**Example:**
```swift
let stack = UIStackView(arrangedSubviews: [icon, title])
stack.axis = .horizontal; stack.spacing = 8; stack.alignment = .center
```

**Interview angle:** Know `distribution` (.fill, .fillEqually, .equalSpacing) vs `alignment`. Stack views don't render anything themselves.

**Resources:** Apple — *UIStackView*; Use Your Loaf articles.

---

## 5.5 UITableView (reuse, delegate/datasource, diffable)

**Summary (easy):** Table views show scrolling lists efficiently by *reusing* cells (only ~visible ones exist). The datasource supplies cells/data; the delegate handles interaction. Diffable data sources animate changes safely.

**Example:**
```swift
func tableView(_ t: UITableView, cellForRowAt ip: IndexPath) -> UITableViewCell {
    let cell = t.dequeueReusableCell(withIdentifier: "Cell", for: ip)
    cell.textLabel?.text = items[ip.row]
    return cell
}
```

**Interview angle:** Explain cell reuse and why you must reset cell state in `prepareForReuse`. Know `NSDiffableDataSource` + snapshots over manual `reloadData`.

**Resources:** Apple — *UITableView* & *Diffable Data Source*; WWDC19 "Advances in UI Data Sources".

---

## 5.6 UICollectionView & Compositional Layout

**Summary (easy):** Like a table view but for grids and arbitrary layouts. `UICollectionViewCompositionalLayout` composes items → groups → sections, making complex, responsive layouts declarative.

**Example:**
```swift
let item = NSCollectionLayoutItem(layoutSize: .init(
    widthDimension: .fractionalWidth(0.5), heightDimension: .fractionalHeight(1)))
// item -> group -> section -> layout
```

**Interview angle:** Compositional layout + diffable data source is the modern combo. Be able to sketch a 2-column grid.

**Resources:** Apple — *Compositional Layout*; WWDC19/20 collection view sessions.

---

## 5.7 Responder Chain & Gesture Recognizers

**Summary (easy):** Touch/events travel up a chain of responders (view → superview → VC → window → app) until something handles them. Gesture recognizers (tap, pan, pinch) detect higher-level gestures.

**Example:**
```swift
let tap = UITapGestureRecognizer(target: self, action: #selector(handleTap))
view.addGestureRecognizer(tap)
```

**Interview angle:** "How does `becomeFirstResponder` relate to the keyboard?" and "how do events propagate?" are common.

**Resources:** Apple — *Using Responders and the Responder Chain*.

---

## 5.8 Navigation, Presentation & Programmatic vs Storyboard UI

**Summary (easy):** `UINavigationController` manages a push/pop stack; modals are *presented*. UI can be built in Storyboards, XIBs, or fully in code — each with trade-offs.

**Example:**
```swift
navigationController?.pushViewController(DetailVC(), animated: true)
present(SettingsVC(), animated: true)
```

**Interview angle:** "Storyboards vs programmatic?" → storyboards: visual, but merge conflicts & hard to reuse; programmatic: verbose but explicit, reviewable, testable. Have an opinion.

**Resources:** Apple — *UINavigationController*; common "no storyboard" guides.

---

## 5.9 App & Scene Lifecycle, Run Loop

**Summary (easy):** An app moves through states (not running → inactive → active → background → suspended). Since iOS 13, `UIScene` supports multiple windows. The run loop keeps the main thread alive and processes events/timers.

**Example:**
```swift
func sceneDidBecomeActive(_ scene: UIScene) { resumeWork() }
func sceneDidEnterBackground(_ scene: UIScene) { saveState() }
```

**Interview angle:** Know where to save state (entering background), and the difference between `AppDelegate` and `SceneDelegate` responsibilities.

**Resources:** Apple — *Managing Your App's Life Cycle*; WWDC "Architecting Your App for Multiple Windows".
