---
tags:
  - Database
  - Postgres
---

```bash
select pg_relation_size(oid), relname from pg_class order by pg_relation_size(oid) desc;
```