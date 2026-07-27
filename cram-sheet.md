#  — MCQ Cram Sheet

Rapid-fire facts. Optimized for recognition, not depth. Read Day 1, re-read Day 5.

---

## 1. OOP

**Four pillars**
- Encapsulation — bundling data + methods, hiding internal state behind an interface.
- Abstraction — exposing behaviour, hiding implementation.
- Inheritance — deriving a class from another (is-a).
- Polymorphism — one interface, many implementations. Compile-time (overloading) vs runtime (overriding).

**Overloading vs overriding**

| | Overloading | Overriding |
|---|---|---|
| Binding | Compile-time (static) | Runtime (dynamic) |
| Signature | Must differ | Must be identical |
| Scope | Same class | Parent → child |
| Return type | Can differ | Must be same or covariant |

**Abstract class vs interface**
- Abstract class: can have state, constructors, implemented methods. Single inheritance (in Java/C#).
- Interface: contract only (pre-Java 8). Multiple implementation allowed.
- Rule of thumb: abstract class = "is-a" with shared code; interface = "can-do" capability.

**Other terms**
- **Static** members belong to the class, not the instance. No `this`.
- **Composition over inheritance** — favour has-a; inheritance couples subclass to parent internals.
- **Coupling** low = good. **Cohesion** high = good.
- **Association / Aggregation / Composition** — plain link / has-a with independent lifetime / has-a with dependent lifetime (delete parent → delete child).
- **Method hiding** — a static method redeclared in a subclass hides, does not override.
- **Constructor chaining** — child constructor implicitly calls parent's no-arg constructor first.
- **Shallow copy** copies references; **deep copy** copies the objects too.

**SOLID**
- **S**ingle Responsibility — one reason to change.
- **O**pen/Closed — open for extension, closed for modification.
- **L**iskov Substitution — subtype must be usable wherever base type is.
- **I**nterface Segregation — many small interfaces > one fat one.
- **D**ependency Inversion — depend on abstractions, not concretions.

---

## 2. Design Patterns

**Creational**
- **Singleton** — one instance, global access. (Logger, DB connection pool.)
- **Factory Method** — subclass decides which class to instantiate.
- **Abstract Factory** — factory of related factories; families of objects.
- **Builder** — step-by-step construction of a complex object.
- **Prototype** — create by cloning an existing instance.

**Structural**
- **Adapter** — converts one interface into another. (Wrapping a legacy API.)
- **Decorator** — adds behaviour at runtime by wrapping. (Express middleware.)
- **Facade** — simplified front over a complex subsystem.
- **Proxy** — placeholder controlling access (lazy loading, access control, caching).
- **Composite** — treat individual objects and groups uniformly (tree structures).
- **Bridge** — decouple abstraction from implementation so both can vary.
- **Flyweight** — share common state across many objects to save memory.

**Behavioural**
- **Observer** — one-to-many notification on state change. (Event emitters, pub/sub.)
- **Strategy** — interchangeable algorithms selected at runtime.
- **Command** — encapsulate a request as an object (undo/redo, queues).
- **Iterator** — sequential access without exposing the underlying structure.
- **Template Method** — skeleton in base class, steps overridden by subclass.
- **State** — object changes behaviour when internal state changes.
- **Chain of Responsibility** — pass request along a chain of handlers.
- **Mediator** — central object coordinating communication between components.
- **Memento** — capture and restore an object's state.

**Architectural (often lumped in)**
- **MVC** — Model (data/logic), View (UI), Controller (input handling).
- **MVVM** — View binds to ViewModel via data binding.
- **Repository** — abstraction layer over data access.
- **Dependency Injection** — supplying dependencies from outside (a form of IoC).

---

## 3. Data Structures — Complexity

| Structure | Access | Search | Insert | Delete | Space |
|---|---|---|---|---|---|
| Array | O(1) | O(n) | O(n) | O(n) | O(n) |
| Sorted array | O(1) | O(log n) | O(n) | O(n) | O(n) |
| Dynamic array | O(1) | O(n) | O(1) amortized (end) | O(n) | O(n) |
| Singly linked list | O(n) | O(n) | O(1)* | O(1)* | O(n) |
| Doubly linked list | O(n) | O(n) | O(1)* | O(1)* | O(n) |
| Stack / Queue | O(n) | O(n) | O(1) | O(1) | O(n) |
| Hash table | — | O(1) avg, O(n) worst | O(1) avg | O(1) avg | O(n) |
| BST (balanced) | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
| BST (skewed) | O(n) | O(n) | O(n) | O(n) | O(n) |
| Binary heap | O(1) min/max | O(n) | O(log n) | O(log n) | O(n) |
| Trie | O(m) | O(m) | O(m) | O(m) | O(ALPHABET·n·m) |
| B-tree | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |

\* given a pointer to the node; O(n) if you must find it first.

**Key facts**
- Stack = LIFO. Queue = FIFO. Deque = both ends. Priority queue ≠ FIFO, ordered by priority.
- Heap: complete binary tree. Min-heap parent ≤ children. Build-heap is O(n), not O(n log n).
- BST in-order traversal → sorted output.
- Balanced BSTs: AVL (strictly balanced, faster reads), Red-Black (looser, faster inserts).
- Hash collisions: chaining (linked lists in buckets) vs open addressing (linear/quadratic probing, double hashing). Load factor triggers resize.
- Graph representations: adjacency matrix O(V²) space, O(1) edge lookup; adjacency list O(V+E) space.

**Traversals**
- Pre-order: root → left → right. In-order: left → root → right. Post-order: left → right → root.
- BFS uses a **queue**, DFS uses a **stack** (or recursion). Both O(V+E) on adjacency lists.

**Graph algorithms**
- Dijkstra — shortest path, non-negative weights, O((V+E) log V) with a heap.
- Bellman-Ford — handles negative weights, detects negative cycles, O(VE).
- Floyd-Warshall — all pairs, O(V³).
- Kruskal (O(E log E), uses union-find) and Prim (O(E log V)) — minimum spanning tree.
- Topological sort — DAGs only, O(V+E).

---

## 4. Sorting

| Algorithm | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | No |
| Counting | O(n+k) | O(n+k) | O(n+k) | O(k) | Yes |
| Radix | O(nk) | O(nk) | O(nk) | O(n+k) | Yes |

- Quicksort worst case = already-sorted input with naive pivot. Fixed by random/median-of-three pivot.
- Merge sort is preferred for linked lists (no random access needed) and external sorting.
- Comparison sorts have a Ω(n log n) lower bound. Counting/radix/bucket beat it by not comparing.

---

## 5. Complexity & Recursion

**Growth order:** O(1) < O(log n) < O(√n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)

- Big-O = upper bound, Ω = lower bound, Θ = tight bound.
- Drop constants and lower-order terms: O(3n² + 5n) = O(n²).
- Nested loop over the same n → O(n²). Loop halving n each step → O(log n).
- **Amortized** — average over a sequence of operations (dynamic array push).

**Master theorem** for T(n) = aT(n/b) + O(n^d):
- if d > log_b(a) → O(n^d)
- if d = log_b(a) → O(n^d log n)
- if d < log_b(a) → O(n^(log_b a))

Examples: binary search T(n)=T(n/2)+O(1) → O(log n). Merge sort T(n)=2T(n/2)+O(n) → O(n log n).

**Recursion**
- Needs a base case + a step that moves toward it. Missing base case → stack overflow.
- Recursion depth costs O(depth) stack space.
- **Tail recursion** — recursive call is the last operation; can be optimized to a loop (not by V8, note).
- Naive Fibonacci = O(2ⁿ); with memoization = O(n).
- Tower of Hanoi = 2ⁿ − 1 moves.
- Every recursion can be rewritten iteratively with an explicit stack.

---

## 6. Databases

**Normalization**
- **1NF** — atomic values, no repeating groups.
- **2NF** — 1NF + no partial dependency on part of a composite key.
- **3NF** — 2NF + no transitive dependency (non-key depending on non-key).
- **BCNF** — every determinant is a candidate key.
- Denormalization trades write consistency for read speed.

**ACID**
- **Atomicity** — all or nothing.
- **Consistency** — valid state to valid state.
- **Isolation** — concurrent transactions don't interfere.
- **Durability** — committed data survives a crash.

**Isolation levels** (weakest → strongest) and what each allows:

| Level | Dirty read | Non-repeatable read | Phantom read |
|---|---|---|---|
| Read Uncommitted | Yes | Yes | Yes |
| Read Committed | No | Yes | Yes |
| Repeatable Read | No | No | Yes |
| Serializable | No | No | No |

**Keys**
- Super key → any set uniquely identifying a row. Candidate key → minimal super key. Primary key → chosen candidate key, NOT NULL + unique. Foreign key → references another table's PK. Composite key → multiple columns.

**Joins**
- INNER — matching rows only. LEFT — all left + matched right (NULLs otherwise). RIGHT — mirror. FULL OUTER — everything. CROSS — Cartesian product. SELF — table joined to itself.

**Indexes**
- Default structure: B-tree (balanced, supports range queries). Hash indexes: equality only.
- Clustered index defines physical row order — one per table. Non-clustered = separate structure pointing at rows.
- Indexes speed reads, slow writes, consume space.
- Composite index (a, b) helps queries on `a` and `a,b` — not on `b` alone (leftmost-prefix rule).
- A function on an indexed column in the WHERE clause usually kills index use.

**SQL execution order:** FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT

- WHERE filters rows before grouping; HAVING filters after.
- DELETE (DML, row by row, rollback-able, fires triggers) vs TRUNCATE (DDL, deallocates pages, resets identity) vs DROP (removes the table).
- UNION removes duplicates; UNION ALL does not (and is faster).
- NULL comparisons need `IS NULL`, not `= NULL`. COUNT(*) counts NULLs; COUNT(col) does not.

**SQL vs NoSQL**
- SQL: fixed schema, strong consistency, joins, vertical scaling.
- NoSQL: flexible schema, horizontal scaling, eventual consistency. Types: document (MongoDB), key-value (Redis), column (Cassandra), graph (Neo4j).
- **CAP theorem** — under a network partition you choose Consistency or Availability. You cannot have all three.

---

## 7. Operating Systems

**Process vs thread**
- Process: own address space, heavier, IPC needed to communicate.
- Thread: shares the process's memory/heap, own stack + registers, lighter context switch.

**Process states:** New → Ready → Running → Waiting/Blocked → Terminated

**Scheduling**
- FCFS — simple, convoy effect.
- SJF — optimal average wait, requires knowing burst time, risks starvation.
- Round Robin — time quantum, good for time-sharing, no starvation.
- Priority — starvation risk, fixed by aging.
- Preemptive vs non-preemptive: whether the OS can forcibly take the CPU back.

**Deadlock — four necessary conditions (all must hold)**
1. Mutual exclusion
2. Hold and wait
3. No preemption
4. Circular wait

Handling: prevention (break a condition), avoidance (Banker's algorithm), detection + recovery, or ignore (ostrich).

**Sync primitives**
- Mutex — locking, ownership, binary. Semaphore — signalling, counting, no ownership.
- Race condition — output depends on thread timing. Critical section — code needing exclusive access.
- Starvation — a process never gets resources. Livelock — processes change state but make no progress.

**Memory**
- Paging — fixed-size pages, causes **internal** fragmentation.
- Segmentation — variable-size logical segments, causes **external** fragmentation.
- Virtual memory — disk as memory extension. Page fault when a page isn't in RAM.
- Page replacement: FIFO (Belady's anomaly), LRU, Optimal (theoretical).
- Thrashing — excessive paging, CPU utilization collapses.
- TLB — cache for page-table lookups.

---

## 8. Networking

**OSI (7):** Physical → Data Link → Network → Transport → Session → Presentation → Application
**TCP/IP (4):** Link → Internet → Transport → Application

Layer → protocol/device:
- Physical: cables, hubs, repeaters
- Data Link: MAC, switches, bridges, Ethernet, ARP
- Network: IP, ICMP, routers
- Transport: TCP, UDP
- Application: HTTP, FTP, SMTP, DNS, SSH, DHCP

**TCP vs UDP**

| | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed, retransmits | Best-effort |
| Ordering | Yes | No |
| Speed | Slower | Faster |
| Header | 20 bytes | 8 bytes |
| Use | HTTP, email, file transfer | DNS, video streaming, gaming, VoIP |

- **TCP 3-way handshake:** SYN → SYN-ACK → ACK. Teardown is 4-way (FIN/ACK ×2).
- Flow control = sliding window. Congestion control = slow start, AIMD.

**Common ports:** 20/21 FTP, 22 SSH, 23 Telnet, 25 SMTP, 53 DNS, 80 HTTP, 110 POP3, 143 IMAP, 443 HTTPS, 3306 MySQL, 5432 PostgreSQL, 6379 Redis, 27017 MongoDB

**DNS** — resolves domain to IP. Order: browser cache → OS cache → resolver → root → TLD → authoritative. Records: A (IPv4), AAAA (IPv6), CNAME (alias), MX (mail), TXT, NS.

**Other**
- IPv4 = 32-bit, IPv6 = 128-bit. Private ranges: 10.x, 172.16–31.x, 192.168.x.
- Subnet mask splits network vs host portion. CIDR /24 = 256 addresses (254 usable).
- DHCP assigns IPs dynamically. NAT maps private to public addresses. ARP maps IP → MAC.
- Hub (layer 1, broadcasts) vs switch (layer 2, MAC-based) vs router (layer 3, IP-based).
- Latency = delay; bandwidth = capacity; throughput = actual rate.

---

## 9. HTTP & APIs

**Methods:** GET (safe, idempotent), POST (not idempotent), PUT (idempotent, full replace), PATCH (partial, not necessarily idempotent), DELETE (idempotent), HEAD, OPTIONS (CORS preflight)

**Status codes**
- 1xx informational
- 2xx: 200 OK, 201 Created, 202 Accepted, 204 No Content
- 3xx: 301 Moved Permanently, 302 Found, 304 Not Modified
- 4xx: 400 Bad Request, 401 Unauthorized (not authenticated), 403 Forbidden (authenticated, not allowed), 404 Not Found, 405 Method Not Allowed, 409 Conflict, 422 Unprocessable Entity, 429 Too Many Requests
- 5xx: 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable, 504 Gateway Timeout

**Concepts**
- HTTP is stateless. State via cookies, sessions, tokens.
- HTTP/1.1 keep-alive; HTTP/2 multiplexing + binary framing + header compression; HTTP/3 over QUIC/UDP.
- REST constraints: client-server, stateless, cacheable, uniform interface, layered, code-on-demand (optional).
- REST vs GraphQL: multiple endpoints/over-fetching vs single endpoint/client-specified queries.
- Idempotent = same result if repeated. Safe = no state change.
- WebSocket = full-duplex persistent connection, starts as an HTTP upgrade.

---

## 10. Security

- **XSS** — injecting script into pages. Fix: output encoding, CSP, sanitize input. Stored / reflected / DOM-based.
- **CSRF** — forcing an authenticated user's browser to make a request. Fix: anti-CSRF tokens, SameSite cookies.
- **SQL injection** — untrusted input in a query. Fix: parameterized queries / prepared statements. (ORMs like Prisma parameterize by default; raw queries do not.)
- **Hashing vs encryption** — hashing is one-way (bcrypt, argon2, SHA-256); encryption is reversible with a key.
- **Symmetric** (AES, one shared key, fast) vs **asymmetric** (RSA, public/private key pair).
- **Salt** — random per-user value added before hashing, defeats rainbow tables.
- **TLS handshake** — asymmetric crypto to exchange a symmetric session key, then symmetric for the session. Certificates signed by a CA.
- **Authentication** = who you are. **Authorization** = what you may do.
- **JWT** — header.payload.signature, base64url encoded. Stateless, signed not encrypted (never put secrets in the payload).
- **Sessions** vs JWT — server-side state and easy revocation vs stateless scaling and hard revocation.
- **CORS** — browser same-origin policy relaxation via response headers. Preflight OPTIONS for non-simple requests.
- **OWASP Top 10** themes: broken access control, cryptographic failures, injection, insecure design, misconfiguration, vulnerable components, auth failures, integrity failures, logging failures, SSRF.
- **Principle of least privilege**, **defense in depth**, **rate limiting**, **MITM attack**, **DDoS**.

---

## 11. Web / HTML / CSS

- Semantic tags: `<header> <nav> <main> <article> <section> <aside> <footer>`. Improve accessibility and SEO.
- Block vs inline vs inline-block. `display: none` (removed) vs `visibility: hidden` (space kept).
- Box model: content → padding → border → margin. `box-sizing: border-box` includes padding+border in width.
- Position: static, relative, absolute (nearest positioned ancestor), fixed (viewport), sticky.
- Specificity: inline (1000) > id (100) > class/attribute/pseudo-class (10) > element (1). `!important` overrides all.
- Flexbox = one dimension. Grid = two dimensions.
- `em` relative to parent font size; `rem` relative to root.
- Pseudo-class (`:hover`) vs pseudo-element (`::before`).

**JavaScript**
- `var` (function-scoped, hoisted as undefined), `let`/`const` (block-scoped, temporal dead zone).
- `==` coerces types, `===` does not.
- Closure — a function retaining access to its lexical scope.
- Event loop: call stack → microtasks (promises) → macrotasks (setTimeout). Microtasks drain first.
- Hoisting: declarations lifted, initializations not. Function declarations fully hoisted.
- `this` depends on call site; arrow functions inherit `this` lexically.
- Prototypal inheritance via the prototype chain.
- `null` (intentional absence) vs `undefined` (not assigned). `typeof null === "object"` — a known bug.
- Falsy values: `false, 0, -0, 0n, "", null, undefined, NaN`.
- Shallow copy: spread/`Object.assign`. Deep: `structuredClone`.

---

## 12. Logical Reasoning & Math

**Formulas worth memorizing**
- Percentage change = (new − old)/old × 100
- Successive % change = a + b + ab/100
- Simple interest = PRT/100; Compound = P(1 + R/100)^T − P
- Average speed for equal distances = 2xy/(x+y)
- Work: if A takes `a` days and B takes `b`, together = ab/(a+b)
- Permutations nPr = n!/(n−r)!; Combinations nCr = n!/(r!(n−r)!)
- Probability = favourable/total. P(A or B) = P(A)+P(B)−P(A and B)
- Profit % = (SP − CP)/CP × 100
- Ratio: if a:b = 2:3 and b:c = 4:5, then a:b:c = 8:12:15

**Number series patterns to scan for:** arithmetic difference, geometric ratio, squares/cubes, primes, alternating series, difference-of-differences, ±n where n increments.

**Syllogisms** — draw Venn diagrams. "Some A are B" does not imply "some B are not A". Only conclusions that *must* follow are valid.

**Bit tricks** (common in MCQs)
- `n & (n−1)` clears the lowest set bit; `n & (n−1) == 0` tests power of two.
- `x ^ x = 0`, `x ^ 0 = x` — used to find a single non-duplicate.
- Left shift by k = multiply by 2^k; right shift = integer divide.
- Binary/hex conversion: practice 5 by hand.

---

## Final-day checklist

- [ ] Complexity table memorized cold
- [ ] All four deadlock conditions
- [ ] Normal forms 1–3 with one example each
- [ ] TCP vs UDP table
- [ ] Status codes 200/201/204/301/400/401/403/404/409/429/500/502/503
- [ ] 10 design patterns, one line each
- [ ] SOLID expanded
- [ ] OSI layers in order (mnemonic: **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way)
- [ ] Sorting stability + worst cases
- [ ] Aptitude formulas above
