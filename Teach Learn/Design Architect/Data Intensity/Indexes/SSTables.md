There are also different strategies to determine the order and timing of how SSTables are compacted and merged. 

The most common options are _`size-tiered`_ and  `leveled_compaction`. 

Usercases:

- LevelDB and RocksDB use leveled compaction (hence the name of LevelDB), H p
- Base uses size-tiered, and Cassandra supports both

How ?
- In size-tiered compaction, newer and smaller SSTables are successively merged into older and larger SSTables. 
- In leveled compaction, the key range is split up into smaller SSTables and older data is moved into separate “levels,” which allows the compaction to proceed more incrementally and use less disk space.