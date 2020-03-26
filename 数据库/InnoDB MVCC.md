
# MySQL InnoDB MVCC

MVCC(multiversion concurrency control),多版本并发控制。MVCC通过保存更改之前的旧值信息实现事务的并发和回滚特性。这个信息保存在表空间中一个叫rollback segment的数据结构中。
