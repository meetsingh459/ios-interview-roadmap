[⬅ Back to Roadmap](../README.md)

# Phase 13 — App Lifecycle, Tooling & Release

The "ship it" knowledge. Shows you understand the full product lifecycle, not just writing features.

---

## 13.1 App States & Background Execution

**Summary (easy):** Apps move between not-running, inactive, active, background, and suspended. The system grants limited background time; for longer work you request specific background modes or use background tasks.

**Example:**
```swift
// BGTaskScheduler for deferred refresh
BGTaskScheduler.shared.register(forTaskWithIdentifier: "refresh", using: nil) { task in
    handleRefresh(task as! BGAppRefreshTask)
}
```

**Interview angle:** Know `beginBackgroundTask` (short extension), `BGTaskScheduler` (refresh/processing), and background URLSession (long downloads).

**Resources:** Apple — *Background Tasks*; WWDC "Background execution demystified".

---

## 13.2 Push & Local Notifications

**Summary (easy):** Local notifications are scheduled by the app; remote (push) notifications come from your server via APNs. You register for a device token, send it to your backend, and APNs delivers payloads.

**Example:**
```swift
let content = UNMutableNotificationContent(); content.title = "Reminder"
let req = UNNotificationRequest(identifier: "id", content: content, trigger: nil)
UNUserNotificationCenter.current().add(req)
```

**Interview angle:** Know the APNs flow, silent/background pushes (`content-available`), and rich/actionable notifications.

**Resources:** Apple — *UserNotifications*; *Setting up a remote notification server*.

---

## 13.3 Deep Linking & Universal Links

**Summary (easy):** Custom URL schemes (`myapp://`) and Universal Links (`https://...` mapped to the app via an Apple App Site Association file) open specific screens directly from links/notifications.

**Example:**
```swift
func scene(_ s: UIScene, continue activity: NSUserActivity) {
    if let url = activity.webpageURL { route(to: url) }
}
```

**Interview angle:** Know why Universal Links are preferred (secure, fall back to web). Know how to parse and route a link to deep state.

**Resources:** Apple — *Universal Links*; *Defining a custom URL scheme*.

---

## 13.4 Code Signing, Provisioning & Certificates

**Summary (easy):** Apple requires apps to be cryptographically signed. A certificate proves who you are; a provisioning profile ties your app ID + devices + entitlements together. This is what lets the app run on real devices and ship.

**Example (concept):**
```
Certificate (identity) + App ID + Devices + Entitlements = Provisioning Profile
```

**Interview angle:** Be able to explain development vs distribution profiles and common "code signing" failures. You don't need to memorize every step, just the model.

**Resources:** Apple — *Code Signing*; Fastlane match docs.

---

## 13.5 CI/CD (Fastlane, Xcode Cloud)

**Summary (easy):** Continuous Integration builds/tests every change; Continuous Delivery automates beta/App Store releases. Fastlane scripts the tedious steps; Xcode Cloud is Apple's hosted CI.

**Example (concept):**
```ruby
# Fastlane lane
lane :beta do
  build_app(scheme: "App")
  upload_to_testflight
end
```

**Interview angle:** Describe a pipeline: PR → run tests/lint → build → TestFlight → App Store. Mention signing automation and secrets handling.

**Resources:** Fastlane docs; Apple — *Xcode Cloud*.

---

## 13.6 App Store, TestFlight, Crash Reporting & Feature Flags

**Summary (easy):** TestFlight distributes betas; App Store Connect handles submission and review. Crash reporting (MetricKit/Crashlytics/Sentry) surfaces production issues; feature flags let you toggle/roll out features without a new release.

**Example (concept):**
```swift
if FeatureFlags.shared.isEnabled(.newCheckout) { showNewCheckout() }
```

**Interview angle:** Know phased rollouts, staged release, symbolication of crash logs, and why feature flags + remote config reduce release risk.

**Resources:** Apple — *App Store Connect*, *MetricKit*; common crash-reporting SDK docs.

---

## 13.7 App Thinning, Bitcode & App Size

**Summary (easy):** App thinning delivers only the resources a given device needs (app slicing), plus on-demand resources and (historically) bitcode. Keeping app size down improves install/conversion rates.

**Example (concept):**
```
App Store ▸ slicing → device-specific IPA; asset catalogs enable per-device assets
```

**Interview angle:** Know that bitcode is now deprecated; focus on asset catalogs, on-demand resources, and analyzing the size report.

**Resources:** Apple — *App Thinning*; WWDC app size sessions.
