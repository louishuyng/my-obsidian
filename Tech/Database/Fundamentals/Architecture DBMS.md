---
tags:
  - Database
  - Fundamental
---
Database management systems use a _client/server model_, where database system instances (_nodes_) take the role of servers, and application instances take the role of clients.

![dbin 0101](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781492040330/files/assets/dbin_0101.png)

### Transport
Client requests arrive through the _transport_ subsystem. Requests come in the form of queries, most often expressed in some query language. The transport subsystem is also responsible for communication with other nodes in the database cluster.
### Query Processor
Upon receipt, the transport subsystem hands the query over to a _query processor_, which parses, interprets, and validates it. Later, access control checks are performed, as they can be done fully only after the query is interpreted.

The parsed query is passed to the _query optimizer_, which first eliminates impossible and redundant parts of the query, and then attempts to find the most efficient way to execute it based on internal statistics (index cardinality, approximate intersection size, etc.) and data placement (which nodes in the cluster hold the data and the costs associated with its transfer). The optimizer handles both relational operations required for query resolution, usually presented as a dependency tree, and optimizations, such as index ordering, cardinality estimation, and choosing access methods.
### Execution Engine
The query is usually presented in the form of an _execution plan_ (or _query plan_): a sequence of operations that have to be carried out for its results to be considered complete. Since the same query can be satisfied using different execution plans that can vary in efficiency, the optimizer picks the best available plan.

The execution plan is carried out by the _execution engine_, which aggregates the results of local and remote operations. _Remote execution_ can involve writing and reading data to and from other nodes in the cluster, and replication.

### Local queries (coming directly from clients or from other nodes) are executed by the _storage engine_. The storage engine has several components with dedicated responsibilities:

#### Transaction manager

This manager schedules transactions and ensures they cannot leave the database in a logically inconsistent state.

#### Lock manager

This manager locks on the database objects for the running transactions, ensuring that concurrent operations do not violate physical data integrity.

#### Access methods (storage structures)

These manage access and organizing data on disk. Access methods include heap files and storage structures such as B-Trees or LSM Trees.

#### Buffer manager

This manager caches data pages in memory.

Recovery manager

This manager maintains the operation log and restoring the system state in case of a failure.

Together, transaction and lock managers are responsible for concurrency control: they guarantee logical and physical data integrity while ensuring that concurrent operations are executed as efficiently as possible.