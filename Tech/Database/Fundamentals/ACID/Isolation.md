---
tags: Database, Fundamental
---
## Read phenomena
### Dirty reads
![[Pasted image 20230816211925.png]]
### Non-repeatable reads
![[Pasted image 20230816213137.png]]
### Phantom reads
![[Pasted image 20230816213909.png]]

### Lost updates
![[Pasted image 20230816235224.png]]

## Isolation Levels
### Read uncommitted
No isolation, any change from the outside is visible to the transaction, committed or not

### Read committed
Each query in transaction only sees committed changes by other transaction

### Repeatable Read
The transaction will make sure that when a query reads a row, that row will remain unchanged the transaction while its running

### Snapshot
Each query transaction only see changes that have been committed up to the start of the transaction. It's like a snapshot version of the database at the moment

### Serializer
Transactions are run as if they serialized one after the other


## Read Phenomena vs Isolation Levels
![[Pasted image 20230817213516.png]]