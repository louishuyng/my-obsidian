---
tags: Database, Performance
---
```bash
create table temp(t int);

insert into temp(t) select random() * 100 from generate_series(0, 100000
 0);
```