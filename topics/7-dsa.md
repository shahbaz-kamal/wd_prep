# 7. Data Structures & Algorithms

## Data Structures — Detailed Explanations

### Array (static, fixed size)

**What it is:** A fixed-size collection of elements stored one after another in **contiguous memory**, each addressable by an integer index. Size must be known at creation and cannot change.

- **Access — O(1):** An array stores elements in **contiguous memory**. Given the base address, an element is found with one arithmetic formula: `base + index × element_size`. No traversal is needed — the CPU jumps straight to the address. This is the key advantage of arrays over linked structures.
- **Search — O(n):** There is no ordering to exploit, so finding a value requires a linear scan. In the worst case the element is missing or at the end, so you inspect all n elements.
- **Insert — O(n):** To insert at position i you must shift every element from i onward one slot right to make room. Inserting at the front shifts all n elements (worst case).
- **Delete — O(n):** Removing an element leaves a hole that must be filled by shifting every subsequent element one slot left.
- **Space — O(n):** Exactly n slots for n elements; no extra pointers or metadata. The trade-off is that size is fixed at creation.

### Sorted array

**What it is:** An array whose elements are kept in a defined order (ascending or descending). The ordering is the only difference from a plain array, and it unlocks faster search at the cost of slower inserts/deletes.

- **Access — O(1):** Same contiguous-memory formula as a plain array.
- **Search — O(log n):** Now the data is ordered, so **binary search** applies. Each comparison discards half the remaining range, so you need only ~log₂ n steps regardless of n.
- **Insert — O(n):** Binary search finds the insertion point in O(log n), but inserting still requires shifting elements to keep the array sorted — O(n) worst case.
- **Delete — O(n):** Finding the element is O(log n), but shifting to close the gap is O(n).
- **Space — O(n):** Same as a plain array. The cost of sorted order is slow inserts/deletes, the benefit is fast search.

### Dynamic array (e.g., Java ArrayList, C++ vector, Python list)

**What it is:** An array that grows and shrinks automatically. When full, it allocates a larger backing array (typically 2×) and copies elements over, so you get array-like O(1) access without a fixed size.

- **Access — O(1):** Same contiguous-memory indexing as a static array.
- **Search — O(n):** Unsorted, so linear scan.
- **Insert — O(1) amortized (at end):** When capacity runs out, the array is **doubled** and everything is copied (a rare O(n) event). Spreading that one big copy over all the cheap inserts makes the *average* cost O(1) — this is what "amortized" means. Inserting in the middle is still O(n) because of shifting.
- **Delete — O(n):** Shifting to close the hole; if the array shrinks (some implementations halve at a low load factor), that rare copy is also amortized.
- **Space — O(n):** May hold up to ~2n slots due to spare capacity reserved for future growth.

### Singly linked list

**What it is:** A chain of nodes in **non-contiguous memory**, where each node holds a value and a single `next` pointer to the following node. There is no index — you can only reach nodes by walking from the head.

- **Access — O(n):** Nodes live in **non-contiguous memory**, connected by pointers. The only way to reach node i is to walk from the head, following `next` pointers one at a time. No address arithmetic is possible.
- **Search — O(n):** Same reason — a linear walk from the head.
- **Insert — O(1)\*:** Given a **pointer to a node**, inserting after it is a constant number of pointer re-assignments (new node's `next` → old next; old node's `next` → new node). The \* is the catch: finding that node first costs O(n).
- **Delete — O(1)\*:** Given the pointer, skip it by re-pointing the previous node's `next` to its successor. Again, you need the node first (or use a "copy the next node's data" trick).
- **Space — O(n):** n elements plus one `next` pointer per node — more memory per element than an array, and pointer overhead hurts cache locality.

### Doubly linked list

**What it is:** A linked list where each node holds a value plus **two pointers** — `next` (to the following node) and `prev` (to the preceding one). This enables constant-time traversal and removal in both directions, at the cost of extra memory.

- **Access — O(n):** Same pointer-walk limitation as a singly linked list.
- **Search — O(n):** Same linear walk (can start from either head or tail, but still O(n)).
- **Insert — O(1)\*:** With a node pointer you can insert *before or after* it in constant time — the `prev` pointer makes backward re-linking trivial (a singly linked list needs the predecessor, which costs a walk).
- **Delete — O(1)\*:** The `prev` pointer lets you reach the predecessor directly, so removing a known node is constant time without a search.
- **Space — O(n):** Two pointers (`prev` + `next`) per node — the most memory-heavy of the pointer-based lists. Used for deques and LRU caches where you need fast operations at both ends.

### Stack / Queue

**What they are:** Abstract data types defined by their restricted access rules, not by how they store data. A **stack** is **LIFO** (Last-In, First-Out) — you push and pop from the top. A **queue** is **FIFO** (First-In, First-Out) — you enqueue at the rear and dequeue from the front. Both are usually built on an array or a linked list.

- **Access — O(n):** There is no indexed access at all. A stack exposes only the **top**; a queue only the **front/rear**. Reaching a middle element requires popping/dequeuing past it.
- **Search — O(n):** To find a value you must pop/dequeue elements one by one — a linear scan that also destroys the structure unless you restore it.
- **Insert — O(1):** Stack `push` adds to the top; queue `enqueue` adds to the rear. Both are constant time (implemented over an array or linked list).
- **Delete — O(1):** Stack `pop` removes the top; queue `dequeue` removes the front. Constant time.
- **Space — O(n):** Stores n elements. The value of these structures is their **restricted interface** (LIFO/FIFO), not their speed — they guarantee exactly one cheap operation at each end.

### Hash table

**What it is:** A structure that maps **keys to values** by passing each key through a **hash function** to compute a bucket index in an array. The goal is near-constant-time lookup without needing the data sorted or searched linearly.

- **Access — "—":** You cannot access by index. Lookup is by **key**, not position — so the access column does not apply. The hash function maps the key directly to a bucket.
- **Search — O(1) average, O(n) worst:** `hash(key) % table_size` jumps straight to a bucket; within a bucket you check for the key. O(1) when buckets hold ~1 item. Worst case, **many collisions** degenerate all keys into one bucket, making it a linear scan.
- **Insert — O(1) average:** Compute the hash, write into the bucket. When the **load factor** (items/buckets) passes a threshold, the table **resizes** (typically doubling) and every element is re-hashed — an occasional O(n) event, amortized to O(1).
- **Delete — O(1) average:** Same direct bucket lookup, then remove. Caveat: open-addressing schemes often need a "tombstone" marker instead of a hard delete to keep probes working.
- **Space — O(n):** n entries plus the bucket array, which is kept larger than n (load factor < 1). Collision handling: **chaining** (linked list per bucket) vs **open addressing** (linear/quadratic probing, double hashing).

### BST (balanced — AVL / Red-Black)

**What it is:** A binary search tree where each node has at most two children, all keys in the left subtree are smaller and all in the right subtree are larger. **Balanced** means a self-adjusting invariant keeps the height ≈ log n — AVL enforces strict balance, Red-Black a looser one.

- **Access — O(log n):** "Access" means searching for a value. At each node you compare and go left or right, discarding the other subtree — the tree height is kept at ~log n by rotations, so every path costs O(log n).
- **Search — O(log n):** Same reason. Balanced height guarantees it.
- **Insert — O(log n):** Descend the tree to the leaf position (O(log n)), then **rebalance** with rotations (AVL) or color fixes (Red-Black) — each also O(log n).
- **Delete — O(log n):** Find the node, splice out (O(log n) with successor replacement), then rebalance (O(log n)).
- **Space — O(n):** n nodes plus left/right pointers and a balance/color flag per node. AVL is stricter (tighter balance, faster reads, costlier inserts); Red-Black is looser (faster inserts, slightly deeper).

### BST (skewed / unbalanced)

**What it is:** A binary search tree that has lost its balance — e.g., after inserting keys in sorted order — so every node ends up on the same side and the tree degenerates into a linked list with height n.

- **All operations — O(n):** If elements are inserted in sorted order (e.g., 1,2,3,…), every node lands on the same side and the "tree" degenerates into a **linked list**. Each operation then walks the full height n — no better than a list. This is why self-balancing variants exist.

### Binary heap

**What it is:** A **complete binary tree** (all levels full except possibly the last) stored implicitly in an array. In a **min-heap** every parent ≤ its children (max-heap: parent ≥ children), so the smallest/largest element is always at the root. It powers priority queues and heapsort.

- **Access — O(1):** The min (min-heap) or max (max-heap) is **always at the root**, stored at index 0 of the backing array — a single read. There is no other indexed access.
- **Search — O(n):** The heap property only guarantees parent ≤ children; it says nothing about ordering *across* siblings, so finding an arbitrary value requires a full scan.
- **Insert — O(log n):** Add the new element at the end of the array (keeping it a complete tree), then **bubble up**: swap with parents until the heap property holds. Path length is the tree height = log n.
- **Delete — O(log n):** Remove the root, move the last element to the root, then **bubble down**/sift: swap with the smaller (min-heap) child until order is restored. This is the basis of heapsort and of priority-queue pop.
- **Space — O(n):** Stored implicitly in an array (index math: parent = i/2, children = 2i, 2i+1) — no pointers. **Build-heap** is O(n), not O(n log n), because most nodes only sift a short distance.

### Trie (prefix tree)

**What it is:** A tree where each edge is labeled with a character and each node represents a prefix of the stored keys. Shared prefixes are stored once, making it ideal for prefix-based lookups. Inserting "cat" and "car" shares the "ca" nodes.

- **All operations — O(m), where m = key length:** Every operation walks the key character by character, following one edge per character. The cost depends on the *length of the key*, not the number of stored keys — so searching for a 10-character word in a million-word dictionary is 10 steps.
- **Access/Search — O(m):** Walk characters from the root; a missing edge means "not found."
- **Insert — O(m):** Walk existing edges, create missing ones for remaining characters, mark the end of the key.
- **Delete — O(m):** Walk to the end, unmark, then prune nodes with no children and no other keys below.
- **Space — O(ALPHABET · n · m):** Worst case, each node may hold up to one child per alphabet character, and the tree can have n·m nodes (n keys × m chars). This is the trie's weakness — it can blow up in memory. Compressed tries (radix / Patricia) mitigate this. Great for autocomplete, spell-check, and IP routing.

### B-tree

**What it is:** A self-balancing tree where each node stores **many keys** and has many children (a high branching factor), so the tree stays very short. Each node is sized to fit one disk page — designed for databases and filesystems.

- **All operations — O(log n):** A B-tree node holds **many keys** (not just one), so its branching factor is large and the tree height stays tiny even for millions of records — height ≈ log_branching(n).
- **Access/Search — O(log n):** At each node do a small binary search among its keys, then follow the correct child pointer. Log n with a very small constant in practice.
- **Insert — O(log n):** Find the leaf, insert, and **split** a full node into two — the split propagates upward at most to the root, keeping the tree balanced automatically.
- **Delete — O(log n):** Find, remove, and fix via redistribution/merging of nodes to keep the "each node ≥ half full" invariant.
- **Space — O(n):** Designed for **disk/DB storage** — each node maps to one disk page, so the small height means few disk I/Os per operation. This is why databases and filesystems (e.g., MySQL, ext4) use B-trees, not binary trees.

---

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
