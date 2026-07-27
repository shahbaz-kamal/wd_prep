# 7. Data Structures & Algorithms

## Data Structures — Complexity

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

## Sorting

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

## Complexity & Recursion

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
