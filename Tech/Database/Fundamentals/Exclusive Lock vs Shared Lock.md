---
tags:
  - Database
  - Fundamental
---

|S.No.|Shared Lock|Exclusive Lock|
|---|---|---|
|1.|Lock mode is read only operation.|Lock mode is read as well as write operation.|
|2.|Shared lock can be placed on objects that do not have an exclusive lock already placed on them.|Exclusive lock can only be placed on objects that do not have any other kind of lock.|
|3.|Prevents others from updating the data.|Prevents others from reading or updating the data.|
|4.|Issued when transaction wants to read item that do not have an exclusive lock.|Issued when transaction wants to update unlocked item.|
|5.|Any number of transaction can hold shared lock on an item.|Exclusive lock can be hold by only one transaction.|
|6.|S-lock is requested using lock-S instruction.|X-lock is requested using lock-X instruction.|
|7.|Example: Multiple transactions reading the same data|Example: Transaction updating a table row|