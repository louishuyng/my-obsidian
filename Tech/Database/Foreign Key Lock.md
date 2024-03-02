---
tags: Database, Research
---
Khi Thêm/xoá Foreign Key thì DB sẽ lock write mấy cái bảng liên quan đến FK đó, sau khi add xong thì DB sẽ thực hiện thêm  1 bước validate toàn bộ data trong các bảng đó. Nên khi nào traffic cao, hoặc làm việc trên bảng có số lượng record lớn, mà thêm/xoá FK thì likely là bị SRE/DBA lấy dép vả vô mặt.

Có nhiều cách để handle vụ này, ví dụ như:i
1. Add FK vào lúc traffic thấp
2. Hoặc là disable FK check đi rồi add, nếu xài MySQL.  

```sql
SET FOREIGN_KEY_CHECKS = 0;
ALTER TABLE child_table ADD FOREIGN KEY (parent_id) REFERENCES parent_table(id);
SET FOREIGN_KEY_CHECKS = 1;
```
3. Nếu xài Postgres thì add FK kèm theo lệnh `NOT VALID`, để disable validation  

```sql
ALTER TABLE child_table ADD FOREIGN KEY (parent_id) REFERENCES parent_table(id) NOT VALID;
```

Cách này giải quyết tạm thời vấn đề chậm hoặc traffic cao. Sau khi hành sự xong thì phải validate lại data cho chắc ăn, trong Postgres thì có thể dùng:
```sql
ALTER TABLE child_table ADD FOREIGN KEY (parent_id) REFERENCES parent_table(id) NOT VALID;
```