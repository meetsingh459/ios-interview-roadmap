[⬅ Back to Roadmap](../README.md)

# Phase 9 — Persistence & Data Storage

You'll be asked "where would you store X?" — and the right answer depends on size, sensitivity, and structure. Know the whole toolbox.

---

## 9.1 UserDefaults

**Summary (easy):** A simple key-value store for small bits of data (settings, flags, last-opened tab). Persists across launches. *Not* for large or sensitive data.

**Example:**
```swift
UserDefaults.standard.set(true, forKey: "hasOnboarded")
let seen = UserDefaults.standard.bool(forKey: "hasOnboarded")
```

**Interview angle:** Never store tokens/passwords here (it's a plist, unencrypted). Don't store large blobs — it's loaded into memory.

**Resources:** Apple — *UserDefaults*; Use Your Loaf articles.

---

## 9.2 Keychain

**Summary (easy):** The secure, encrypted store for sensitive data: passwords, tokens, keys. Backed by the Secure Enclave-protected keychain services.

**Example:**
```swift
// Use a wrapper like KeychainAccess, or Security framework SecItemAdd directly
try keychain.set(accessToken, key: "auth_token")
```

**Interview angle:** Explain *why* tokens go in the Keychain, not UserDefaults. Know accessibility options (e.g., `.whenUnlockedThisDeviceOnly`).

**Resources:** Apple — *Keychain Services*; KeychainAccess library.

---

## 9.3 File System (FileManager)

**Summary (easy):** Store files in the app sandbox: `Documents` (user data, backed up), `Caches` (regenerable, may be purged), `tmp` (temporary). Use `FileManager` to read/write/move.

**Example:**
```swift
let url = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
    .appendingPathComponent("notes.json")
try data.write(to: url)
```

**Interview angle:** Know the directories and their backup/purge behavior. Large downloads → Caches or Application Support, not Documents.

**Resources:** Apple — *File System Programming Guide*.

---

## 9.4 Core Data (stack, contexts, fetch, concurrency)

**Summary (easy):** Apple's object-graph persistence framework. The "stack" = managed object model + persistent store coordinator + managed object context(s). You fetch/insert/save through a context.

**Example:**
```swift
let request = NSFetchRequest<Note>(entityName: "Note")
request.predicate = NSPredicate(format: "isPinned == YES")
request.sortDescriptors = [NSSortDescriptor(key: "date", ascending: false)]
let notes = try context.fetch(request)
```

**Interview angle:** Concurrency is the deep question: use a background context for heavy work and merge into the main (view) context. Know `NSFetchedResultsController`, faulting, and lightweight migration.

**Resources:** Apple — *Core Data Programming Guide*; objc.io "Core Data" book.

---

## 9.5 SwiftData (iOS 17+)

**Summary (easy):** A modern, Swift-native persistence layer built on Core Data, using the `@Model` macro. Far less boilerplate; integrates cleanly with SwiftUI via `@Query`.

**Example:**
```swift
@Model class Note { var title: String; init(title: String) { self.title = title } }
// In a View:  @Query var notes: [Note]
```

**Interview angle:** Know SwiftData is built atop Core Data and when you'd still reach for Core Data (fine-grained control, older OS support).

**Resources:** WWDC23 "Meet SwiftData"; Apple — *SwiftData*; Hacking with Swift SwiftData course.

---

## 9.6 SQLite / GRDB / Realm (awareness)

**Summary (easy):** SQLite is the underlying database many tools wrap. GRDB is a fast, type-safe Swift SQLite toolkit. Realm is an alternative object database. Useful when you need raw SQL power or cross-platform sync.

**Example (concept):**
```swift
// GRDB: try dbQueue.write { db in try note.insert(db) }
```

**Interview angle:** Be able to compare: Core Data/SwiftData (Apple-native, object graph) vs GRDB/SQLite (closer to SQL, predictable) vs Realm (easy, but a third-party runtime).

**Resources:** GRDB GitHub docs; Realm docs; SQLite.org.

---

## 9.7 Migration Strategies

**Summary (easy):** When your data model changes between app versions, you migrate existing user data. Core Data offers lightweight (automatic) and heavyweight (mapping model) migrations; SwiftData uses `VersionedSchema` + `SchemaMigrationPlan`.

**Example (concept):**
```swift
// Core Data lightweight migration options:
options = [NSMigratePersistentStoresAutomaticallyOption: true,
           NSInferMappingModelAutomaticallyOption: true]
```

**Interview angle:** Migrations are a real production pain point — showing you've thought about versioning and not losing user data scores points.

**Resources:** Apple — *Core Data Model Versioning and Data Migration*; SwiftData schema migration docs.
