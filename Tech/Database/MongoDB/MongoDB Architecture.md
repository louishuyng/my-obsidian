---
tags:
  - Database
  - MongoDB
  - NoSQL
---
Source from **husseinmac** His youtube here: https://www.youtube.com/@hnasr

#### Original MongoDB Architecture

When Mongo first released, it used a storage engine called [MMAPV1](https://www.mongodb.com/docs/v3.2/core/mmapv1/) which stands for Memory Map files. In MMAPV1 BSON documents are stored directly on disk uncompressed, and the _id primary key index maps to a special value called Diskloc. Diskloc is a pair of 32 bit integers representing the file number and the file offset on disk where the document lives.

When you fetch a document using its _id, the B+Tree primary key index is used to find the Diskloc value which is used to read the document directly from disk using the file and offset.

As you might have guessed, MMAPV1 comes with some of limitations. While Diskloc is an amazing O(1) way to find the document from disk using the file and offset, maintaining it is difficult as documents are inserted and updated. When you update a document, the size increases changing the offset values, which means now all Diskloc offsets after that document are now off and need to be updated. Another major limitation with MMapv1 is the single global databse lock for writes, which means only 1 writer per database can write at at time, signfically slowing down concurrent writes.

> In this image you can see the leaf pages of the B+Tree are linked together for effective range scans, this is a property of B+Trees.

![](https://www.udemy.com/course/database-engines-crash-course/learn/)One index lookup is required in this architecture plus an I/O 𝑂(𝑙𝑜𝑔𝑛) + 𝑂(1)

Mongo did improve MMapv1 to make it a collection level lock (table level lock) but later depreceated MMapv1 in 4.0 in favor of WiredTiger, their new and default storage engine.

#### MongoDB WiredTiger Architecture

In 2014. MongoDB [acquired](https://www.mongodb.com/press/wired-tiger) WiredTiger and made it their default storage engine. WiredTiger has many features such as document level locking and compression. This allowed two concurrent writes to update different documents in the same collection without being serialized, something that wasn’t possible in the MMAPV1 engine. BSON documents in WiredTiger are compressed and stored in a hidden index where the leaf pages are recordId, BSON pairs. This means more BSON documents can be fetched with fewer I/Os making I/O in WiredTiger more effective increasing the overall performance.

Primary index _id and secondary indexes have been changed to point to recordId (a 64 bit integer) instead of the Diskloc. This is a similar model to PostgreSQL where all indexes are secondary and point directly to the tupleid on the heap.

However, this means if a user looks up the _id for a document, Mongo uses the primary index to find the recordId and then does another lookup on the hidden WT index to find the BSON document. The same goes for writes, inserting a new document require updating two indexes.

![](https://www.udemy.com/course/database-engines-crash-course/learn/)Two look ups are required in this architecture 𝑂(𝑙𝑜𝑔𝑛) + 𝑂(𝑙𝑜𝑔𝑛)

The double lookup cost consumes CPU, memory, time and disk space to store both primary index and the hidden clustered index. This is also true for secondary indexes. I can’t help but remember a [blog article](https://discord.com/blog/how-discord-stores-billions-of-messages) by Discord why they moved away from Mongo to Cassandra, one reason was their data files and indexes [could no longer fit RAM](https://discord.com/blog/how-discord-stores-billions-of-messages). The storage of both indexes might have exacerbated their problem but I could be wrong.

Despite the extra I/O and double index storage for _id in this architecture, both primary and secondary indexes are still predictable in size. The recordId is a 64 bit which is relatively small. Keep this in mind because this is going to change one more time.

#### Clustered Collections Architecture

Clustered collections is a brand new feature in Mongo introduced in [June 2022](https://jira.mongodb.org/browse/SERVER-14569). A Clustered Index is an index where a lookup gives you all what you need, all fields are stored in the the leaf page resulting in what is commonly known in database systems as Index-only scans. Clustered collections were introduced in Mongo 5.3 making the primary _id index a clustered index where leaf pages contain the BSON documents and no more hidden WT index.

This way a lookup on the _id returns the BSON document directly, improving performance for workloads using the _id field. No more second lookup.

![](https://www.udemy.com/course/database-engines-crash-course/learn/)Only one index lookup is required in this architecture 𝑂(𝑙𝑜𝑔𝑛) against the _id index

Because the data has technically moved, secondary indexes need to point to _id field instead of the recordId. This means secondary indexes will still need to do two lookups one on the secondary index to find the _id and another lookup on the primary index _id to find the BSON document. Nothing new here as this is what they used to do in non-clustered collections, except we find recordid instead of _id.

However this creates a problem, the secondary index now stores 12 bytes (yes bytes not bits) as value to their key which significantly bloats all secondary indexes on clustered collection. What makes this worse is some users might define their own _id which can go beyond 12 bytes further exacerbating the size of secondary indexes. So watch out of this during data modeling.

This changes Mongo architecture to be similar to MySQL InnoDB, where secondary indexes point to the primary key. But unlike MySQL where tables [MUST](https://dev.mysql.com/doc/refman/5.7/en/innodb-index-types.html) be clusetered, in Mongo at least get a choice to cluster your collection or not. This is actually pretty good tradeoff.