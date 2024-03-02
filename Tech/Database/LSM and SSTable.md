---
tags:
  - Database
  - Fundamental
---

Source: https://wei-ming.tw/posts/sstable-and-lsmtree
#### Landscape of database storages

[![Landscape of database storages](https://wei-ming.tw/static/b0453fbb95c32586f445dca4f9795c72/d9199/storage-engine-tree.png "Landscape of database storages")](https://wei-ming.tw/static/b0453fbb95c32586f445dca4f9795c72/3e992/storage-engine-tree.png)

In short, database storages are divided into two categories, row-based and column based. We will talk more about the difference of them in future article. In this article, we will talk more about the log-structure storage under row-based storage. And in next article, we will talk more about page-oriented storage engine.

## [](https://wei-ming.tw/posts/sstable-and-lsmtree#log-structured-storage-engine)Log-structured storage engine

### [](https://wei-ming.tw/posts/sstable-and-lsmtree#high-level-concept)High level concept

[![how log structure storage works](https://wei-ming.tw/static/df9e4d7ea5a3d27c05202cefb51cb500/d9199/log-structure-storage.png "how log structure storage works")](https://wei-ming.tw/static/df9e4d7ea5a3d27c05202cefb51cb500/d3deb/log-structure-storage.png)

- Every time when there is an new write query, we append the data to the end of file.
- When file size reach certain threshold (e.g. several MB), we append data to another new file. We call each file as “segment”.
- Compact & Merge the files at some points in background process.

#### [](https://wei-ming.tw/posts/sstable-and-lsmtree#very-first-version-of-our-own-db)Very first version of our own DB

The concept sounds very simple, let’s write a simple version of DB by using shell script.

```bash
#!/bin/bash

db_set () {
    echo "$1,$2" >> database
}

db_get () {
    grep "^$1," database | sed -e "s/^$1,//" | tail -n 1
}
```

```bash
$ db_set 123456 '{"name":"London","attractions":["Big Ben","London Eye"]}'

$ db_set 42 '{"name":"San Francisco","attractions":["Golden Gate Bridge"]}'

$ db_get 42
{"name":"San Francisco","attractions":["Golden Gate Bridge"]}
```

It works! Wow, we already write our first very simple database. So, let’s think about pros and cons of this kind of databases.

- Pros: very good write performance.
- Cons: while searching data, we need to search from newest segment, one file after another. The performance could be worse if we search old data.
- Then, how can we improve the read performance ?

#### [](https://wei-ming.tw/posts/sstable-and-lsmtree#enhancement-adding-index)Enhancement: adding index

Now one bottleneck is, we need to search the key one by one in a segment file. The time complexity is O(n). If we ever learned any programming language, we can intuitively think of the hashmap data structure (e.g. dict in python, object in javascript).

If we add a hashmap for each segment, maintain the position of value for each key. Then we can reduce the searching performance around O(log n). This is actually what [Bitcask](https://github.com/basho/bitcask) did.

#### [](https://wei-ming.tw/posts/sstable-and-lsmtree#how-it-works)How it works:

[![log structure storage engine with hashmap](https://wei-ming.tw/static/77b810dcf665996bd187b758f033410b/d9199/log-structure-storage-with-hash.png "log structure storage engine with hashmap")](https://wei-ming.tw/static/77b810dcf665996bd187b758f033410b/a8a6f/log-structure-storage-with-hash.png)

- For write request:
    
    - Update key value in hashmap, append data to log file.
    - When file is large enough, write both hashmap & data as file.
- For read request:
    
    - Load hashmap from newest segment, search whether the key is in hashmap.
    - If not found, load next segment, and do the same thing.

#### [](https://wei-ming.tw/posts/sstable-and-lsmtree#discussions)Discussions

Okay, let’s stop here for a while. What is the pros and cons of this kind of database ? Can we make it even better ?

### [](https://wei-ming.tw/posts/sstable-and-lsmtree#sstables--lsm-tree)SSTables & LSM Tree

Yes, here comes SSTables and LSM Tree.

Let’s do almost exactly the same logic we mentioned above (log + index), but with a change: **the data in each segment file are sorted by keys**

#### [](https://wei-ming.tw/posts/sstable-and-lsmtree#the-benefits-of-sorted-data-in-segment-file)The benefits of sorted data in segment file

- Merge several segment files can be very efficient by merge sort algorithm, even for very large file that cannot be loaded into memory.
- Since segment data was sorted, we don’t need to store all key value pairs, we can keep sparse index and do binary search.

#### [](https://wei-ming.tw/posts/sstable-and-lsmtree#how-it-works-1)How it works

[![Memtable and SSTables](https://wei-ming.tw/static/71ed0488ba6f8432ad60effe1c923651/d9199/LSM-tree.png "Memtable and SSTables")](https://wei-ming.tw/static/71ed0488ba6f8432ad60effe1c923651/add4c/LSM-tree.png)

- For write request:
    
    - Update key value in a balance tree, append data to log file (only for recovery)
    - When the tree is large enough, write the data in key order as a segment file
    - only need to kept very sparse index for each segment since segment file is sorted
- For read request:
    
    - try to search it from memtable first
    - if not found, search the newest SSTable
    - do binary search via sparse index
    - extract entire small block from disk into memory, if not found, search next next SSTable
- Background process:
    
    - merge several SSTables(segment file) by merge sort
- For index:
    
    - We can have smaller index data stored in disk.
    - For example if there are 10,000 key-value pairs in one segment file, we can only sparsely store 100 key-pairs in our index only. Then after doing binary search, we can locate the possible 100 key-value pairs in segment file.
    - Because OS load file into memory by fixed size block, there isn’t much difference of I/O performance comparing to only load 1 key-value pair (disk I/O is usually the bottleneck of database performance). And scan 100 key-value in memory can still be very efficient.
    - So, to sum up, more sparse index, we can save more disk space, but the searching performance can be worse.

#### [](https://wei-ming.tw/posts/sstable-and-lsmtree#terms)Terms

- SSTables (Sorted String Tables) → the data stored in segment was sorted
- LSM Trees (Log Structured Merge tree) → balanced tree in memory, sorted log file in disk
- Real world databases use the concept of LSM tree & SSTables: [Cassandra](https://cassandra.apache.org/_/index.html) / [LevelDB](https://github.com/google/leveldb) / [HBase](https://hbase.apache.org/)
- Full-text search engine: [Lucence](https://lucene.apache.org/) / [Solr](https://solr.apache.org/) / [ElasticSearch](https://www.elastic.co/)