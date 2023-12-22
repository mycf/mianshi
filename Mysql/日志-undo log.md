# 基本概念

事务提交后并不能马上删除undo log及undo log所在的页。这是因为可能还有其他事务需要通过undo log来得到行记录之前的版本。故事务提交时将undo log放入一个链表中，是否可以最终删除undo log及undo log所在页由purge线程来判断。

# 作用
## 提供回滚操作（实现了原子性）
事务可以通过undo log将数据回滚到修改之前的样子。

undo是逻辑日志，因此只是将数据库逻辑地恢复到原来的样子。所有修改都被逻辑地取消了，但是数据结构和页本身在回滚之后可能大不相同。

*逻辑日志*：可以认为对于每个INSERT，InnoDB存储引擎会记录一个DELETE；对于每个DELETE，InnoDB存储引擎会记录一个INSERT；对于每个UPDATE，InnoDB存储引擎会记录一个相反的UPDATE，将修改前的行放回去。


## MVCC
即在InnoDB存储引擎中MVCC的实现是通过undo来完成。当用户读取一行记录时，若该记录已经被其他事务占用，当前事务可以通过undo读取之前的行版本信息，以此实现非锁定读取。

**注意：undo log会产生redo log，也就是undo log的产生会伴随着redo log的产生，这是因为undo log也需要持久性的保护。**
