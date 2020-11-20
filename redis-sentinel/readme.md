# Redis 集群 主从架构  添加 哨兵



在Redis容器内部启动sentinel即可，三个容器分别进入启动哨兵。  执行：



`redis-sentinel sentinel.conf`





启动成功之后，如果我们down掉redis1这个容器，集群会自动选举redis2或者redis3为Master.

