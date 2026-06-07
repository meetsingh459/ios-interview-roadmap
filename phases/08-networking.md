[⬅ Back to Roadmap](../README.md)

# Phase 8 — Networking

Nearly every app talks to a server. Expect questions on URLSession, parsing, error handling, auth, and security.

---

## 8.1 URLSession Basics (data / download / upload tasks)

**Summary (easy):** `URLSession` is Apple's networking API. A *data task* fetches data into memory; a *download task* streams to a file; an *upload task* sends a body. Sessions can be `.shared`, default, ephemeral, or background.

**Example:**
```swift
let (data, response) = try await URLSession.shared.data(from: url)
guard let http = response as? HTTPURLResponse, http.statusCode == 200 else { throw NetError.bad }
```

**Interview angle:** Know session types (ephemeral = no disk cache/cookies; background = continues when app is suspended). Know `URLRequest` configuration (headers, method, body, timeout).

**Resources:** Apple — *URLSession*; Donny Wals — "URLSession" series.

---

## 8.2 REST, HTTP Methods & Status Codes

**Summary (easy):** REST APIs use HTTP verbs (GET read, POST create, PUT/PATCH update, DELETE remove) and status codes (2xx success, 3xx redirect, 4xx client error, 5xx server error) over resource URLs.

**Example:**
```swift
var req = URLRequest(url: url)
req.httpMethod = "POST"
req.setValue("application/json", forHTTPHeaderField: "Content-Type")
req.httpBody = try JSONEncoder().encode(payload)
```

**Interview angle:** Know idempotency (GET/PUT/DELETE idempotent, POST not) and common codes (401 vs 403, 429 rate limit).

**Resources:** MDN HTTP docs; Apple URL Loading System guide.

---

## 8.3 JSON Parsing with Codable + Error Handling

**Summary (easy):** Decode server JSON into Swift models with `JSONDecoder` and `Codable`. Wrap calls so network, decoding, and server errors are handled distinctly.

**Example:**
```swift
enum APIError: Error { case transport(Error), decoding(Error), server(Int) }
func fetchUsers() async throws -> [User] {
    let (data, resp) = try await URLSession.shared.data(from: url)
    guard let code = (resp as? HTTPURLResponse)?.statusCode, 200..<300 ~= code
    else { throw APIError.server((resp as? HTTPURLResponse)?.statusCode ?? -1) }
    do { return try JSONDecoder().decode([User].self, from: data) }
    catch { throw APIError.decoding(error) }
}
```

**Interview angle:** Distinguish error categories and design a clean error enum — interviewers love seeing thoughtful error modeling.

**Resources:** Apple — *Fetching website data into memory*; Swift by Sundell — networking + Codable.

---

## 8.4 Authentication (tokens, OAuth, JWT, refresh)

**Summary (easy):** Most APIs use bearer tokens in an `Authorization` header. OAuth issues access + refresh tokens; JWTs are self-contained signed tokens. When an access token expires (401), you use the refresh token to get a new one and retry.

**Example:**
```swift
req.setValue("Bearer \(accessToken)", forHTTPHeaderField: "Authorization")
```

**Interview angle:** Be ready to describe a token-refresh flow that retries the original request, and to store tokens in the Keychain (not UserDefaults).

**Resources:** Apple — *Authenticating with a server*; Auth0 OAuth/JWT explainers.

---

## 8.5 Caching (URLCache) & Conditional Requests

**Summary (easy):** `URLCache` caches responses in memory/disk based on HTTP cache headers (`Cache-Control`, `ETag`). Conditional requests (`If-None-Match`) let the server reply 304 "not modified" to save bandwidth.

**Example:**
```swift
let config = URLSessionConfiguration.default
config.requestCachePolicy = .returnCacheDataElseLoad
```

**Interview angle:** Know the difference between HTTP caching and your own app-level cache, and cache invalidation strategies.

**Resources:** Apple — *Accessing cached data*; RFC 7234 (caching) overview.

---

## 8.6 WebSockets & Streaming

**Summary (easy):** For real-time data (chat, live scores), `URLSessionWebSocketTask` keeps a persistent two-way connection so the server can push messages.

**Example:**
```swift
let task = URLSession.shared.webSocketTask(with: wsURL)
task.resume()
task.send(.string("hello")) { _ in }
```

**Interview angle:** Compare polling vs long-polling vs WebSockets vs server-sent events; know reconnection/heartbeat concerns.

**Resources:** Apple — *URLSessionWebSocketTask*; WWDC "Advances in Networking".

---

## 8.7 Networking Libraries & Security (Alamofire, certificate pinning)

**Summary (easy):** Alamofire wraps URLSession with conveniences (chaining, validation, interceptors). Certificate/public-key pinning hardens against man-in-the-middle attacks by trusting only known server certs. Always use HTTPS (ATS enforces it).

**Example (concept):**
```swift
// URLSessionDelegate.urlSession(_:didReceive:completionHandler:) -> validate server trust
```

**Interview angle:** Be able to justify URLSession vs Alamofire ("don't add a dependency for what URLSession + async/await already does well"). Explain App Transport Security (ATS).

**Resources:** Alamofire docs; OWASP Mobile — pinning; Apple — App Transport Security.
