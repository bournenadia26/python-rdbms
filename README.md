# python-rdbms

A from-scratch implementation of L-Store, a lineage-based relational database engine,
built incrementally across three milestones for ECS 165A: Database Systems (UC Davis,
Winter 2026). L-Store combines transactional and analytical workloads in a single engine
using a novel base/tail page architecture that separates original records from updates.

*Group project. Original repo is private due to academic integrity policy. This is a
personal fork.*

Primary contributions: disk persistence, full database open/close lifecycle management, and two-phase locking concurrency control.

---

## What We Built

A fully functional, ACID-compliant, multi-threaded relational database in pure Python,
built across three milestones:

**Milestone 1 — In-Memory L-Store**
Core data model and query interface from scratch. Columnar base/tail page architecture
with a page directory mapping RIDs to physical page locations. Full SQL-like query
support: insert, select, update, delete, and range-sum aggregation.

**Milestone 2 — Durability & Bufferpool**
Disk persistence via binary I/O for page data and pickled serialization for metadata,
including full open/close database lifecycle. Bufferpool with dirty page tracking,
pin/unpin semantics, and page eviction. Background merge process that
lazily consolidates tail records into base pages using Tail-Page Sequence Numbers
(TPS) for contention-free operation. Secondary indexing on any column.

**Milestone 3 — Multithreaded Transactions**
Full ACID transaction semantics with atomicity and rollback on abort. Two-phase locking
(2PL) with no-wait abort semantics to eliminate deadlocks, applied across records,
bufferpool, and indexes. Fixed-size worker thread pool for concurrent transaction
execution.

## Architecture
```
Database
└── Table
    ├── Page Directory (RID → physical location)
    ├── Page Ranges
    │   ├── Base Pages (read-optimized, columnar)
    │   └── Tail Pages (write-optimized, append-only)
    └── Bufferpool
```

---

## Stack

Python, threading, io
