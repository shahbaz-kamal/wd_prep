# 4. Databases

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
