# Practice MCQ Bank

100 questions across the four advertised sections. Answer key with explanations at the end.

**How to use:** Attempt Section 1–2 after Day 2, Section 3 after Day 4, Section 4 on Day 5. Time yourself at **45 seconds per question**. Score below 70% in a section → re-read that part of the cram sheet.

---

## Section A — Programming & OOP (Q1–25)

**1.** Which is resolved at compile time?
a) Method overriding b) Method overloading c) Virtual function dispatch d) Interface implementation

**2.** A class that cannot be instantiated but may contain implemented methods is:
a) Interface b) Final class c) Abstract class d) Static class

**3.** SOLID's "D" stands for:
a) Decomposition b) Delegation Principle c) Dependency Inversion d) Data Hiding

**4.** Which pattern ensures exactly one instance of a class exists?
a) Factory b) Singleton c) Prototype d) Builder

**5.** Express middleware chaining is closest to which pattern?
a) Observer b) Decorator/Chain of Responsibility c) Singleton d) Memento

**6.** Which violates the Liskov Substitution Principle?
a) A subclass adding a new method
b) A subclass overriding a method to throw NotSupportedException
c) A subclass calling super()
d) A subclass with a different constructor

**7.** In `a = b = 5`, assignment associativity is:
a) Left to right b) Right to left c) Undefined d) Depends on the compiler

**8.** Which is NOT a pillar of OOP?
a) Inheritance b) Polymorphism c) Compilation d) Encapsulation

**9.** Composition implies:
a) is-a relationship b) has-a with dependent lifetime c) uses-a d) is-implemented-by

**10.** A static method belongs to:
a) Each instance b) The class c) The superclass d) The interface

**11.** Which pattern would you use to add logging to an object at runtime without modifying its class?
a) Adapter b) Facade c) Decorator d) Bridge

**12.** In JavaScript, `typeof null` returns:
a) "null" b) "undefined" c) "object" d) "number"

**13.** What does a closure capture?
a) A copy of variable values b) The lexical scope by reference c) The global object d) Only function parameters

**14.** Output order: `console.log(1); setTimeout(()=>console.log(2),0); Promise.resolve().then(()=>console.log(3)); console.log(4);`
a) 1 2 3 4 b) 1 4 3 2 c) 1 4 2 3 d) 1 3 4 2

**15.** Which is falsy in JavaScript?
a) "0" b) [] c) {} d) NaN

**16.** `let` differs from `var` primarily in:
a) Type safety b) Block scoping c) Performance d) Immutability

**17.** High cohesion and low coupling generally means:
a) Poor design b) Good design c) Over-engineering d) Tight integration

**18.** The Observer pattern is best described as:
a) One-to-one synchronous call b) One-to-many notification on state change c) Object cloning d) Interface conversion

**19.** Which pattern provides a simplified interface to a complex subsystem?
a) Proxy b) Facade c) Composite d) Flyweight

**20.** Encapsulation is primarily achieved through:
a) Inheritance b) Access modifiers c) Polymorphism d) Interfaces

**21.** In `const obj = {a:1}; obj.a = 2;` — the result is:
a) TypeError b) obj.a becomes 2 c) Silently ignored d) ReferenceError

**22.** Which statement about interfaces (classical OOP) is true?
a) They can hold instance state
b) A class may implement multiple interfaces
c) They must have constructors
d) They cannot declare methods

**23.** An arrow function's `this`:
a) Is bound at call time b) Is inherited lexically c) Is always global d) Is always undefined

**24.** Dependency Injection is a form of:
a) Inversion of Control b) Reflection c) Serialization d) Memoization

**25.** Which is an example of runtime polymorphism?
a) Two methods with different parameter lists
b) A subclass overriding a parent method, called via a parent reference
c) Generic type parameters
d) A static factory method

---

## Section B — DSA & Complexity (Q26–50)

**26.** Deleting the last element of a singly linked list of size n, given only the head:
a) O(1) b) O(log n) c) O(n) d) O(n²)

**27.** Worst-case time of quicksort:
a) O(n log n) b) O(n²) c) O(n) d) O(log n)

**28.** Which sort is stable and O(n log n) in the worst case?
a) Quicksort b) Heapsort c) Merge sort d) Selection sort

**29.** Building a heap from an unsorted array takes:
a) O(n) b) O(n log n) c) O(log n) d) O(n²)

**30.** In-order traversal of a BST yields:
a) Reverse sorted b) Sorted ascending c) Level order d) Random order

**31.** BFS uses which structure?
a) Stack b) Queue c) Heap d) Trie

**32.** T(n) = 2T(n/2) + O(n) solves to:
a) O(n) b) O(n log n) c) O(n²) d) O(log n)

**33.** Average lookup in a hash table with good hashing:
a) O(1) b) O(log n) c) O(n) d) O(n log n)

**34.** Which algorithm handles negative edge weights?
a) Dijkstra b) Bellman-Ford c) Prim d) BFS

**35.** Space complexity of merge sort:
a) O(1) b) O(log n) c) O(n) d) O(n log n)

**36.** A complete binary tree with n nodes has height:
a) n b) ⌊log₂ n⌋ c) n/2 d) √n

**37.** Which is NOT a comparison sort?
a) Heapsort b) Radix sort c) Insertion sort d) Merge sort

**38.** Naive recursive Fibonacci has time complexity:
a) O(n) b) O(n²) c) O(2ⁿ) d) O(n log n)

**39.** Topological sorting applies to:
a) Any graph b) Undirected graphs c) Directed acyclic graphs d) Weighted graphs only

**40.** Adjacency matrix space complexity:
a) O(V+E) b) O(V²) c) O(E) d) O(V log V)

**41.** A missing base case in recursion causes:
a) Infinite loop b) Stack overflow c) Segmentation fault d) Compile error

**42.** Minimum number of moves for Tower of Hanoi with n disks:
a) n² b) 2ⁿ c) 2ⁿ − 1 d) n!

**43.** Which structure gives O(1) access to the minimum element?
a) BST b) Min-heap c) Hash table d) Queue

**44.** Binary search requires:
a) A sorted array b) A hash function c) A linked list d) A balanced tree

**45.** Amortized cost of appending to a dynamic array:
a) O(1) b) O(log n) c) O(n) d) O(n log n)

**46.** `n & (n-1) == 0` tests whether n is:
a) Even b) Prime c) A power of two d) Negative

**47.** Worst case of searching a skewed BST:
a) O(1) b) O(log n) c) O(n) d) O(n log n)

**48.** Which traversal visits root last?
a) Pre-order b) In-order c) Post-order d) Level-order

**49.** Time complexity of a nested loop where the inner loop halves each iteration and the outer runs n times:
a) O(n) b) O(n log n) c) O(n²) d) O(log n)

**50.** Kruskal's algorithm typically uses which auxiliary structure?
a) Priority queue only b) Union-Find (disjoint set) c) Trie d) Hash map

---

## Section C — CS Fundamentals (Q51–80)

**51.** Which is NOT an ACID property?
a) Atomicity b) Availability c) Isolation d) Durability

**52.** Removing transitive dependencies achieves:
a) 1NF b) 2NF c) 3NF d) BCNF

**53.** Which isolation level permits phantom reads but not dirty reads?
a) Read Uncommitted b) Read Committed c) Serializable d) None

**54.** HAVING differs from WHERE because it:
a) Runs before grouping b) Filters aggregated groups c) Is faster d) Only works with joins

**55.** How many clustered indexes can a table have?
a) Zero b) One c) One per column d) Unlimited

**56.** A composite index on (last_name, first_name) will NOT efficiently serve a query filtering only on:
a) last_name b) first_name c) both d) last_name with a range

**57.** TRUNCATE differs from DELETE because it:
a) Can use a WHERE clause b) Is DDL and deallocates pages c) Fires row triggers d) Is slower

**58.** UNION vs UNION ALL:
a) UNION is faster b) UNION ALL removes duplicates c) UNION removes duplicates d) They are identical

**59.** Which returns all rows from the left table regardless of matches?
a) INNER JOIN b) LEFT JOIN c) CROSS JOIN d) SELF JOIN

**60.** CAP theorem states that under a partition you must choose between:
a) Cost and Performance b) Consistency and Availability c) Atomicity and Durability d) Speed and Storage

**61.** Which is NOT a necessary condition for deadlock?
a) Mutual exclusion b) Hold and wait c) Preemption d) Circular wait

**62.** Threads within a process share:
a) Stack b) Registers c) Heap and code segment d) Program counter

**63.** Paging causes which type of fragmentation?
a) External b) Internal c) Both d) Neither

**64.** Which scheduling algorithm minimizes average waiting time?
a) FCFS b) Shortest Job First c) Round Robin d) Priority

**65.** Excessive page faults causing collapsed CPU utilization is called:
a) Thrashing b) Starvation c) Livelock d) Aging

**66.** A mutex differs from a semaphore because it:
a) Is always counting b) Has ownership c) Cannot block d) Is faster by definition

**67.** Belady's anomaly occurs with which replacement policy?
a) LRU b) FIFO c) Optimal d) LFU

**68.** Which OSI layer does a router primarily operate at?
a) Data Link b) Network c) Transport d) Session

**69.** TCP's connection setup is:
a) 2-way handshake b) 3-way handshake c) 4-way handshake d) Connectionless

**70.** Which protocol commonly runs over UDP?
a) HTTP/1.1 b) FTP c) DNS d) SMTP

**71.** Default PostgreSQL port:
a) 3306 b) 5432 c) 6379 d) 27017

**72.** ARP resolves:
a) Domain to IP b) IP to MAC c) MAC to port d) IP to port

**73.** Status code 403 means:
a) Not authenticated b) Authenticated but not permitted c) Resource missing d) Server error

**74.** Which HTTP method is idempotent but not safe?
a) GET b) POST c) PUT d) OPTIONS

**75.** 502 Bad Gateway indicates:
a) Client sent malformed input b) An upstream server returned an invalid response c) Rate limit exceeded d) Resource moved

**76.** Parameterized queries primarily prevent:
a) XSS b) CSRF c) SQL injection d) Clickjacking

**77.** A salt is used to:
a) Encrypt passwords b) Make identical passwords hash differently c) Compress hashes d) Speed up hashing

**78.** A JWT payload is:
a) Encrypted b) Base64url encoded and signed, not encrypted c) Hashed d) Compressed only

**79.** SameSite cookies primarily mitigate:
a) XSS b) CSRF c) SQL injection d) MITM

**80.** TLS uses asymmetric cryptography to:
a) Encrypt all traffic b) Exchange a symmetric session key c) Compress data d) Validate HTML

---

## Section D — Logical Reasoning & Math (Q81–100)

**81.** Next in series: 2, 6, 12, 20, 30, ?
a) 40 b) 42 c) 44 d) 46

**82.** Next: 1, 4, 9, 16, 25, ?
a) 30 b) 36 c) 35 d) 49

**83.** Next: 3, 7, 15, 31, 63, ?
a) 95 b) 127 c) 125 d) 128

**84.** A can finish a job in 12 days, B in 18. Together they take:
a) 6.6 days b) 7.2 days c) 8 days d) 9 days

**85.** A price rises 20% then falls 20%. Net change:
a) 0% b) −4% c) +4% d) −2%

**86.** A car covers half a distance at 40 km/h and half at 60 km/h. Average speed:
a) 50 b) 48 c) 45 d) 52

**87.** In how many ways can 5 people be seated in a row?
a) 25 b) 60 c) 120 d) 720

**88.** Probability of at least one head in two fair coin tosses:
a) 1/2 b) 1/4 c) 3/4 d) 2/3

**89.** If a:b = 3:4 and b:c = 6:7, then a:c is:
a) 9:14 b) 3:7 c) 18:28 d) 9:28

**90.** Selling at 1200 gives 20% profit. Cost price:
a) 960 b) 1000 c) 1050 d) 900

**91.** 15% of 240 is:
a) 32 b) 36 c) 38 d) 40

**92.** Binary 1011 in decimal:
a) 9 b) 11 c) 13 d) 15

**93.** Hexadecimal 2F in decimal:
a) 31 b) 47 c) 45 d) 63

**94.** Statements: All engineers are graduates. Some graduates are managers. Which follows?
a) All engineers are managers b) Some engineers are managers c) No valid conclusion about engineers and managers d) No managers are engineers

**95.** If TODAY is coded as UPEBZ, then CODE is coded as:
a) DPEF b) DPFE c) CPEF d) DOEF

**96.** A is B's father. B is C's sister. C is D's mother. A is D's:
a) Father b) Grandfather c) Uncle d) Great-grandfather

**97.** Odd one out: 8, 27, 64, 100, 125
a) 27 b) 64 c) 100 d) 125

**98.** 5 machines make 5 widgets in 5 minutes. How long for 100 machines to make 100 widgets?
a) 100 min b) 20 min c) 5 min d) 1 min

**99.** Next: 2, 3, 5, 7, 11, 13, ?
a) 15 b) 17 c) 19 d) 21

**100.** If 3x + 7 = 22, then x² =
a) 5 b) 15 c) 25 d) 49

---

# Answer Key

## Section A
1. **b** — overloading binds at compile time; overriding at runtime.
2. **c** — abstract classes may contain concrete methods; interfaces (classically) may not.
3. **c**
4. **b**
5. **b** — each middleware decides whether to pass control onward.
6. **b** — refusing a contract the base type honours breaks substitutability.
7. **b**
8. **c**
9. **b** — composition means the contained object dies with the container.
10. **b**
11. **c**
12. **c** — a long-standing language bug.
13. **b** — by reference, which is why loop-variable closures with `var` surprise people.
14. **b** — sync first (1, 4), then microtask (3), then macrotask (2).
15. **d** — `"0"`, `[]`, and `{}` are all truthy.
16. **b**
17. **b**
18. **b**
19. **b**
20. **b**
21. **b** — `const` binds the reference, not the object's contents.
22. **b**
23. **b**
24. **a**
25. **b**

## Section B
26. **c** — you must traverse to the second-to-last node.
27. **b** — worst case is a consistently bad pivot.
28. **c**
29. **a** — heapify bottom-up is O(n); inserting n elements individually is O(n log n).
30. **b**
31. **b**
32. **b** — merge sort's recurrence.
33. **a**
34. **b**
35. **c**
36. **b**
37. **b** — radix sorts by digit, not by comparison.
38. **c**
39. **c**
40. **b**
41. **b**
42. **c**
43. **b**
44. **a**
45. **a** — doubling makes the cost amortize to constant.
46. **c**
47. **c** — a skewed BST degenerates into a linked list.
48. **c**
49. **b**
50. **b** — to detect cycles while adding edges.

## Section C
51. **b** — Availability belongs to CAP, not ACID.
52. **c**
53. **b**
54. **b**
55. **b** — it defines physical row order, so only one.
56. **b** — leftmost-prefix rule.
57. **b**
58. **c**
59. **b**
60. **b**
61. **c** — *no* preemption is the condition; preemption itself breaks deadlock.
62. **c** — each thread keeps its own stack, registers, and PC.
63. **b** — fixed-size pages waste the tail of the last page.
64. **b** — optimal on average, but requires knowing burst times and risks starvation.
65. **a**
66. **b**
67. **b** — more frames can paradoxically cause more faults.
68. **b**
69. **b** — SYN, SYN-ACK, ACK. Teardown is 4-way.
70. **c**
71. **b**
72. **b**
73. **b** — 401 is unauthenticated; 403 is authenticated but forbidden.
74. **c** — PUT changes state (not safe) but repeating it gives the same result.
75. **b**
76. **c**
77. **b** — defeats rainbow tables and reveals nothing from identical passwords.
78. **b** — never put secrets in a JWT payload.
79. **b**
80. **b** — then symmetric encryption handles the session for speed.

## Section D
81. **b** — n(n+1): 2,6,12,20,30,**42**.
82. **b** — perfect squares.
83. **b** — 2ⁿ⁺¹ − 1, i.e. ×2+1 each step.
84. **b** — (12×18)/(12+18) = 216/30 = 7.2.
85. **b** — 20 − 20 − (400/100) = −4%.
86. **b** — 2xy/(x+y) = 2(40)(60)/100 = 48.
87. **c** — 5! = 120.
88. **c** — 1 − P(no heads) = 1 − 1/4 = 3/4.
89. **a** — a:b:c = 9:12:14, so a:c = 9:14.
90. **b** — 1200/1.2 = 1000.
91. **b** — 36.
92. **b** — 8+0+2+1 = 11.
93. **b** — 2×16 + 15 = 47.
94. **c** — the "some" overlap may exclude engineers entirely.
95. **a** — each letter shifts +1.
96. **b** — A is parent of B and C; C is D's mother, so A is D's grandfather.
97. **c** — the others are perfect cubes (2³, 3³, 4³, 5³).
98. **c** — each machine takes 5 minutes per widget; scaling both sides changes nothing.
99. **b** — primes.
100. **c** — x = 5, x² = 25.

---

## Scoring guide

| Score | Read |
|---|---|
| 90–100 | Solid. Spend remaining time on timed speed drills. |
| 75–89 | Competitive. Target your two weakest sections. |
| 60–74 | Re-read the cram sheet sections you failed, then retake. |
| < 60 | Prioritize CS Fundamentals and DSA — highest question density in these tests. |
