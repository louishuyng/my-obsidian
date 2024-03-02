---
tags:
  - Database
  - Fundamental
---

## Introduction
Daktabase systems do use files for storing the data, but instead of relying on filesystem hierarchies of directories and files for locating records, they compose files using implementation-specific formats. The main reasons to use specialized file organization over flat files are:

Storage efficiency

Files are organjijzed in a way that minimizes storage overhead per stored data record.
j
Access efficiency

Records can be located in the smallest possible number of steps.

Update efficiency
k
Record updates akre performed in a way that minimizes the number of changes on disk.

Database systems store _data records_, consisting of multiple fields, in tables, where each table is usually represented as a separate file. Each record in the table can be looked up using a _search key_. To locate a record, database systems use _indexes_: auxiliary data structures that allow it to efficiently locate data records without scanning an entire table on every access. Indexes are built using a subset of fields identifying the record.

A database system usually separates _data files_ and _index files_: data files store data records, while index files store record metadata and use it to locate records in data files. Index files are typically smaller than the data files. Files are partitioned into _pages_, which typically have the size of a single or multiple disk blocks. Pages can be organized as sequences of records or as a _slotted pages_.

New records (insertions) and updates to the existing records are represented by key/value pairs. Most modern storage systems _do not_ delete data from pages explicitly. Instead, they use _deletion markers_ (also called _tombstones_), which contain deletion metadata, such as a key and a timestamp. Space occupied by the records _shadowed_ by their updates or deletion markers is reclaimed during garbage collection, which reads the pages, writes the live (i.e., nonshadowed) records to the new place, and discards the shadowed ones.

## Data Files

Data files (sometimes called _primary files_) can be implemented as _index-organized tables_ (IOT), _heap-organized tables_ (heap files), or _hash-organized tables_ (hashed files).

Records in heap files are not required to follow any particular order, and most of the time they are placed in a write order. This way, no additional work or file reorganization is required when new pages are appended. Heap files require additional index structures, pointing to the locations where data records are stored, to make them searchable.

In hashed files, records are stored in buckets, and the hash value of the key determines which bucket a record belongs to. Records in the bucket can be stored in append order or sorted by key to improve lookup speed.

Index-organized tables (IOTs) store data records in the index itself. Since records are stored in key order, range scans in IOTs can be implemented by sequentially scanning its contents.

Storing data records in the index allows us to reduce the number of disk seeks by at least one, since after traversing the index and locating the searched key, we do not have to address a separate file to find the associated data record.

When records are stored in a separate file, index files hold _data entries_, uniquely identifying data records and containing enough information to locate them in the data file. For example, we can store file _offsets_ (sometimes called _row locators_), locations of data records in the data file, or bucket IDs in the case of hash files. In index-organized tables, data entries hold actual data records.

## Index Files

An index is a structure that organizes data records on disk in a way that facilitates efficient retrieval operations. Index files are organized as specialized structures that map keys to locations in data files where the records identified by these keys (in the case of heap files) or primary keys (in the case of index-organized tables) are stored.

An index on a _primary_ (data) file is called the _primary index_. In most cases we can also assume that the primary index is built over a primary key or a set of keys identified as primary. All other indexes are called _secondary_.
kk
Secondary indexes can point directly to the data record, or simply store its primary key. A pointer to a data record can hold an offset to a heap file or an index-organized table. Multiple secondary indexes can point to the same record, allowing a single data record to be identified by different fields and located through different indexes. While primary index files hold a unique entry per search key, secondary indexes may hold several entries per search key.

If the order of data records follows the search key order, this index is called _clustered_ (also known as clustering). Data records in the clustered case are usually stored in the same file or in a _clustered file_, where the key order is preserved. If the data is stored in a separate file, and its order does not follow the key order, the index is called _nonclustered_ (sometimes called unclustered).

- a) An index-organized table, where data records are stored directly in the index file.
- b) An index file storing the offsets and a separate file storing data records.

![dbin 0105](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781492040330/files/assets/dbin_0105.png)

### NOTE
Index-organized tables store information in index order and are clustered by definition. Primary indexes are _most often_ clustered. Secondary indexes are nonclustered by definition, since they’re used to facilitate access by keys other than the primary one. Clustered indexes can be both index-organized or have separate index and data files.

Many database systems have an inherent and explicit _primary key_, a set of columns that uniquely identify the database record. In cases when the primary key is not specified, the storage engine can create an _implicit_ primary key (for example, MySQL InnoDB adds a new auto-increment column and fills in its values automatically).

This terminology is used in different kinds of database systems: relational database systems (such as MySQL and PostgreSQL), Dynamo-based NoSQL stores (such as [Apache Cassandra](https://databass.dev/links/119) and in [Riak](https://databass.dev/links/120)), and document stores (such as MongoDB). There can be some project-specific naming, but most often there’s a clear mapping to this terminology.

## Primary Index as an Indirection

There are different opinions in the database community on whether data records should be referenced directly (through file offset) or via the primary key index.[4](https://learning.oreilly.com/library/view/database-internals/9781492040330/ch01.html#idm46466889581384)

Both approaches have their pros and cons and are better discussed in the scope of a complete implementation. By referencing data directly, we can reduce the number of disk seeks, but have to pay a cost of updating the pointers whenever the record is updated or relocated during a maintenance process. Using indirection in the form of a primary index allows us to reduce the cost of pointer updates, but has a higher cost on a read path.

Updating just a couple of indexes might work if the workload mostly consists of reads, but this approach does not work well for write-heavy workloads with multiple indexes. To reduce the costs of pointer updates, instead of payload offsets, some implementations use primary keys for indirection. For example, MySQL InnoDB uses a primary index and performs two lookups: one in the secondary index, and one in a primary index when performing a query. This adds an overhead of a primary index lookup instead of following the offset directly from the secondary index.

- a) Two indexes reference data entries directly from secondary index files.
- b) A secondary index goes through the indirection layer of a primary index to locate the data entries.

![dbin 0106](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781492040330/files/assets/dbin_0106.png)

It is also possible to use a hybrid approach and store both data file offsets and primary keys. First, you check if the data offset is still valid and pay the extra cost of going through the primary key index if it has changed, updating the index file after finding a new offset.