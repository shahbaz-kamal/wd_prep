# 5-Day Study Plan

**Target:** On-site MCQ test, 1 August 2026
**Companion files:** `cram-sheet.md`, `practice-mcqs.md`

---

## Strategy

The MCQ on 1 August is the only near-term gate. Phone screening, technical interviews, and the final round run from 9 August through late October — weeks of runway. So all five days go to the MCQ.

**Your likely gaps as a MERN/Prisma developer:**
- OOP theory and design patterns — JS work rarely exercises classical OOP vocabulary
- Complexity analysis stated formally
- OS fundamentals — no daily exposure
- Networking below the HTTP layer
- Raw SQL, normalization, indexing — Prisma has abstracted this away from you

**Your existing strengths (revise only, don't study):** JavaScript semantics, REST/HTTP, API design, HTML/CSS, general web architecture.

---

## Day 1 — Diagnostic + DSA core

**Morning (45 min): diagnostic.** Take Sections A and B of `practice-mcqs.md` cold, timed at 45 seconds per question. Do not look anything up. The score tells you how to reweight the rest of the week — don't skip this to "save time".

**Rest of day: linear data structures.**
- Arrays, dynamic arrays, strings
- Hash tables — collision handling (chaining vs open addressing), load factor
- Stacks, queues, deques, priority queues
- Singly and doubly linked lists

Focus on **properties and operation complexities**, not implementations. MCQs ask "what is the cost of deleting from the middle of a singly linked list given only the head" — not "write the function."

**Reference:** Cram sheet §3

---

## Day 2 — Complexity, recursion, trees, sorting

**Block 1: complexity.** Big-O/Ω/Θ, growth ordering, amortized analysis, master theorem. Practice reading a code snippet and stating its complexity — the single most common MCQ format in this category.

**Block 2: recursion.** Base cases, stack depth as space cost, recurrence relations, tail recursion, memoization. Know Fibonacci naive vs memoized and Tower of Hanoi = 2ⁿ − 1.

**Block 3: trees and graphs.** BST properties, in/pre/post-order traversals, heaps (build-heap is O(n) — a common trap), tries, AVL vs Red-Black. BFS/DFS and their auxiliary structures. Dijkstra vs Bellman-Ford vs Floyd-Warshall. Kruskal/Prim. Topological sort.

**Block 4: sorting.** Memorize the full table — best/average/worst, space, stability. Know quicksort's worst case and why, and why merge sort suits linked lists.

**Reference:** Cram sheet §3, §4, §5

---

## Day 3 — OOP + design patterns

**Block 1: OOP theory.** Four pillars. Then the distinctions MCQs actually test:
- Overloading vs overriding (full table)
- Abstract class vs interface
- Static vs instance members
- Association vs aggregation vs composition
- Shallow vs deep copy
- Coupling and cohesion

Study these in a **class-based framing (Java/C#)**, not just JavaScript. Question banks are written that way, and JS's prototypal model will mislead you on several questions.

**Block 2: SOLID.** Expand each letter and be able to give one violating example — LSP violations in particular show up often.

**Block 3: design patterns.** Cover all of §2 in the cram sheet. One line each: what it does, when it's used. That is exactly MCQ depth — do not go deeper.

Anchor each to something you've actually built: Express middleware is Chain of Responsibility, an event emitter is Observer, a Prisma client is effectively a Singleton, a service layer over Prisma is Repository.

**Then:** retake Section A of the MCQ bank. You should clear 90%.

**Reference:** Cram sheet §1, §2

---

## Day 4 — CS fundamentals (heaviest day)

This is your weakest block and typically the highest question density on trainee tests. Budget the full day.

**Databases (~3 hrs)**
- Normalization 1NF → 3NF → BCNF, with an example each
- ACID; isolation levels and which anomalies each permits
- Key types: super, candidate, primary, foreign, composite
- Joins — all six types
- Indexes: B-tree vs hash, clustered vs non-clustered, leftmost-prefix rule, write cost
- SQL logical execution order; WHERE vs HAVING; DELETE vs TRUNCATE vs DROP; UNION vs UNION ALL; NULL semantics
- SQL vs NoSQL; CAP theorem

**Write raw SQL by hand today. No Prisma.** Open psql against any edux database and write ten queries with joins, aggregates, and subqueries. This closes your specific gap faster than reading.

**Operating systems (~2 hrs)**
- Process vs thread; what threads share
- Process states; context switching
- Scheduling: FCFS, SJF, Round Robin, Priority — tradeoffs and starvation
- Deadlock: all four conditions, and the handling strategies
- Mutex vs semaphore; race conditions; critical sections
- Paging (internal fragmentation) vs segmentation (external)
- Virtual memory, page faults, replacement policies, Belady's anomaly, thrashing, TLB

**Networking (~1.5 hrs)**
- OSI 7 layers in order + which protocol and device sits at each
- TCP vs UDP full table; 3-way handshake; flow vs congestion control
- DNS resolution order and record types
- Common ports
- IPv4/IPv6, subnetting basics, DHCP, NAT, ARP
- Hub vs switch vs router

**Security (~1 hr)**
- XSS, CSRF, SQL injection — mechanism and mitigation for each
- Hashing vs encryption; symmetric vs asymmetric; salting
- TLS handshake
- Authentication vs authorization; JWT structure and its stateless tradeoff; sessions vs tokens
- CORS and preflight
- OWASP themes

**Then:** take Section C of the MCQ bank.

**Reference:** Cram sheet §6, §7, §8, §9, §10

---

## Day 5 — Aptitude + full revision

**Block 1 (2 hrs): logical reasoning and math.** This is pure speed, and it's where many technically strong candidates lose points. Grind:
- Number series (arithmetic, geometric, squares/cubes, primes, alternating, difference-of-differences)
- Percentages, profit/loss, ratios, averages
- Time-speed-distance, work-rate
- Permutations, combinations, probability
- Syllogisms (Venn diagrams), blood relations, coding-decoding
- Binary/hex conversion and bit tricks

Use IndiaBix or similar for volume. Speed matters more than method elegance.

**Block 2 (2 hrs): two timed full mocks.** Then review **only** your wrong answers. Re-reading what you already know is wasted time on the last day.

**Block 3 (1 hr): final checklist.** Work the checklist at the end of the cram sheet. Anything you can't recall in five seconds gets one more pass.

**Stop by evening.** Sleep matters more than a sixth hour on Day 5.

**Reference:** Cram sheet §11, §12 + final checklist

---

## Test-day tactics

- **Time is the binding constraint, not knowledge.** Mark and skip anything past 60 seconds. Come back only if time remains.
- **Check for negative marking before you start.** If there's none, never leave a blank — eliminate two options and guess.
- **Breadth beats depth.** Trainee MCQs sample widely. Knowing 40 topics shallowly outscores knowing 5 deeply.
- **Read the question fully.** "Which is NOT…" questions are the most common source of avoidable loss.
- Arrive early, bring ID, confirm the venue the night before.

---

## After the MCQ (don't touch this week)

Once 1 August clears, your differentiator is that you already ship production Node/Postgres — most of a trainee pool will be fresh graduates with coursework only.

Prepare these for the technical rounds:
- A 90-second walkthrough of edux: what it does, the schema decisions you made, why
- One real bug you diagnosed end to end
- One performance problem you fixed, with the before/after
- Why you're moving from your current role — framed forward, not as a complaint

Don't rehearse any of it now. Clear 1 August first.
