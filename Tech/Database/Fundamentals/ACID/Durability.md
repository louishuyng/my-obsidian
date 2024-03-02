---
tags: Database, Fundamental
---

## Introduction
- Change made by committed transactions must be persisted in a durable non-volatile storage.
- Durabilities Techniques
	- WAL - Write ahead log
	- Asynchronous snapshot
	- AOF
## WAL
- Writing a lot of data to disk is expensive (indexes, data files, columns, rows, etc...)
- That is why DBMSs persist a compressed version of the changes now as WAL (Write ahead Log segments)
## OS Caches
- A write request in OS usually goes to the OS cache
- When an OS crash, machine restart could lead to loss of data
- Fsync OS command forces writes to always go to disk
