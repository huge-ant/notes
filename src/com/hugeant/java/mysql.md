> PXC集群  
* Percona XtraDB Cluster，只支持innodb，强一致性 全部写成功才返回成功，否则回滚，性能较一般主从复制性能差
* 每个节点均基于MySQL/Percona Server
* 使用了Galera library，一个通用的用于事务型应用的同步、多主复制插件
> 优点  
  PXC的特性和优点：
 ​ 1、同步复制
 ​ 2、支持多主复制
 ​ 3、支持并行复制
 ​ 4、作为高可用方案，相比其他方案其结构和实施相对简单明了
> 缺点  
1） 版本（5.6.20）的复制只支持InnoDB引擎，其他存储引擎的更改不复制。然而，DDL（Data Definition Language） 语句在statement级别被复制，并且，对mysql.*表的更改会基于此被复制。例如CREATE USER...语句会被复制，但是 INSERT INTO mysql.user...语句则不会。（也可以通过wsrep_replicate_myisam参数开启myisam引擎的 复制，但这是一个实验性的参数）。
2）由于PXC集群内部一致性控制的机制，事务有可能被终止，原因如下：集群允许在两个节点上通知执行操作同一行的两个事务，但是只有一个能执行成功，另一个 会被终止，同时集群会给被终止的客户端返回死锁错误(Error: 1213 SQLSTATE: 40001 (ER_LOCK_DEADLOCK)).

3）写入效率取决于节点中最弱的一台，因为PXC集群采用的是强一致性原则，一个更改操作在所有节点都成功才算执行成功

4）所有表都要有主键；

5）不支持LOCK TABLE等显式锁操作；

6）锁冲突、死锁问题相对更多；

7）不支持XA；

8）集群吞吐量/性能取决于短板；

9）新加入节点采用SST时代价高；

10）存在写扩大问题；

11）如果并发事务量很大的话，建议采用InfiniBand网络，降低网络延迟；

事实上，采用PXC的主要目的是解决数据的一致性问题，高可用是顺带实现的。因为PXC存在写扩大以及短板效应，并发效率会有较大损失，类似semi sync replication机制。

# sysbench压测命令

sysbench --mysql-host=ums-mysql.wonlycloud.cn --mysql-port=3306 --mysql-user=root --mysql-password=Wl2016822 \
--mysql-db=testdb --report-interval=10 --time=120 --tables=4 --table-size=10000000 --threads=1 \
/usr/share/sysbench/oltp_read_write.lua prepare

