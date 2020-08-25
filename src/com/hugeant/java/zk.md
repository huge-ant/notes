> CAP
* C:一致性
* A:可用性
* p:分区容错性

> BASE  
* BA:基本可用
* S:软状态（中间状态）
* E:最终一致性

> 分布式一致性算法
* 2PC 3PC
* Paxos--->投票模式
* ZAB--->借鉴paxos，zookeeper使用的算法

> zookeeper特性
* 简单的数据结构:共享的树形结构，类似文件目录，存储于内存中;
* 可以构建集群:3-5台集群，超过半数存活即可对外服务;
* 顺序访问:每个读请求，分配一个全局唯一递增编号;
* 高性能:基于内存。

> zk场景
> zk会话（session）
* 客户端与服务端的一次会话连接，本质是TCP长连接，可进行心跳检测与数据传输
>zk节点
* 持久节点：create /james value 重复无法创建成功（节点名称相同）  
* 临时节点：create -e 客户端断开连接后临时节点被删除；无法创建子节点。
* 持久有序节点：
* 临时有序节点：create -e -s  
有序节点后面会带数字，create -e -s /james value可创建成功（如james0000006）
> ACL权限(策略:用户id:权限)
* 保证用户数据安全
scheme:world（全世界） auth（认证过的用户） digest（密码）ip（指定ip）
id:---auth:(username:password) digest:(username:Base64(SHA1(password))) ip:
permissions:create delete read write admin(设置子节点权限)---c  d  r  w  a

