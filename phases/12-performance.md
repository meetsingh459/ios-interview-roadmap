[⬅ Back to Roadmap](../README.md)

# Phase 12 — Performance & Optimization

Senior interviews probe whether you can *diagnose* and *fix* slowness, not just write features. Know the tools and the common culprits.

---

## 12.1 Instruments (Time Profiler, Allocations, Leaks, Core Animation)

**Summary (easy):** Instruments is Xcode's profiling suite. Time Profiler finds slow code; Allocations/Leaks find memory growth and cycles; Core Animation/Animation Hitches find dropped frames; Energy Log finds battery drains.

**Example (workflow):**
```
Product ▸ Profile (⌘I) → pick Time Profiler → record → find the hot stack frames
```

**Interview angle:** Be able to describe a real debugging session: "users reported jank → I ran Core Animation, saw 12ms+ frames, traced it to image decoding on the main thread."

**Resources:** Apple — *Instruments*; WWDC "Optimize app launch / Demystify and eliminate hitches".

---

## 12.2 Main Thread Hygiene & Avoiding Blocking

**Summary (easy):** The main thread renders UI ~60–120 times/second. Any heavy work there (parsing, image decode, disk/network) causes dropped frames. Move it to a background queue/task and hop back to main only for UI.

**Example:**
```swift
Task.detached { let img = await decode(data); await MainActor.run { imageView.image = img } }
```

**Interview angle:** Know the frame budget (~16ms at 60fps, ~8ms at 120fps). Use Main Thread Checker. This ties directly into Phase 4 (concurrency).

**Resources:** WWDC "Eliminate animation hitches"; Apple performance docs.

---

## 12.3 List/Collection Performance (reuse, prefetching, sizing)

**Summary (easy):** Smooth scrolling needs cheap cells: reuse cells, avoid heavy work in `cellForRow`, cache row heights, and use prefetching/diffable data sources. In SwiftUI, keep `body` light and give rows stable identity.

**Example:**
```swift
func collectionView(_ cv: UICollectionView, prefetchItemsAt paths: [IndexPath]) {
    paths.forEach { imageLoader.prefetch(urls[$0.item]) }
}
```

**Interview angle:** "Your feed stutters when scrolling — how do you fix it?" Walk through profiling → offload decode → prefetch → cache heights.

**Resources:** WWDC collection view performance sessions; Apple table view best practices.

---

## 12.4 Image & Asset Optimization

**Summary (easy):** Images are a top source of memory and CPU cost. Downsample to the display size before showing, decode off the main thread, use appropriate formats (HEIC), and cache decoded images.

**Example:**
```swift
// Downsample with ImageIO instead of UIImage(contentsOfFile:)
let opts = [kCGImageSourceThumbnailMaxPixelSize: maxDimension] as CFDictionary
```

**Interview angle:** Know that a "small" JPEG can balloon in memory once decoded (width × height × 4 bytes). Downsampling is the fix.

**Resources:** WWDC18 "Image and Graphics Best Practices".

---

## 12.5 App Launch Time Optimization

**Summary (easy):** Launch = pre-main (dynamic linking, static initializers) + post-main (your `didFinishLaunching` and first frame). Reduce dynamic frameworks, defer non-critical work, and avoid blocking the first frame.

**Example (concept):**
```swift
// Defer analytics/SDK init off the launch critical path
DispatchQueue.main.async { Analytics.start() }
```

**Interview angle:** Know the target (sub-400ms feel), how to measure with the App Launch instrument, and that too many dynamic frameworks slow pre-main.

**Resources:** WWDC "Optimize App Launch"; Apple launch time docs.

---

## 12.6 Build Time & Compiler Optimization

**Summary (easy):** Slow builds hurt productivity. Reduce type-inference complexity, break up huge files, use modular targets, and understand Whole Module Optimization (WMO) for release builds. The `-Onone` (debug) vs `-O` (release) settings matter.

**Example (concept):**
```
// Add -warn-long-function-bodies and -warn-long-expression-type-checking to find slow-to-compile code
```

**Interview angle:** Mention measuring with build timing summaries and splitting modules to enable parallel/incremental builds.

**Resources:** WWDC "Demystify Swift performance"; Swift compiler optimization docs.
