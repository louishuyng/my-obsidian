---
tags:
  - Database
  - Performance
---


Requirement: Retrieve the top 10 rows, following a set of 100,000 rows, in descending order.

instead of 
```bash
select title from news order by id desc offset 100000 limit 10
```

try this
```bash

select title from news where id < 100000 order by id desc
```