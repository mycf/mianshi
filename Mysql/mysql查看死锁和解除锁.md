# 解除正在[死锁](https://so.csdn.net/so/search?q=%E6%AD%BB%E9%94%81&spm=1001.2101.3001.7020)的状态有两种方法：

第一种：

1.查询是否锁表  
```mysql
show OPEN TABLES where In_use > 0;
```

2.查询进程（如果您有SUPER权限，您可以看到所有线程。否则，您只能看到您自己的线程）  

```mysql
show processlist;
```

3.杀死进程id（就是上面命令的id列）
```mysql
kill id
```


第二种：

1.查看下在锁的事务  

```mysql
SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;
```

2.杀死进程id（就是上面命令的trx_mysql_thread_id列）  
```mysql
kill 线程ID  
```

例子：  
查出死锁进程：SHOW PROCESSLIST  
杀掉进程          KILL 420821;  

其它关于查看死锁的命令：  

1：查看当前的事务  
SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;  

2：查看当前锁定的事务  

SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS;

3：查看当前等锁的事务  
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;