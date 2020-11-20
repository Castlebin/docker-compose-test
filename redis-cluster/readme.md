# redis cluster 集群



```yaml
#docker-compose.yml
version: "3.1"
services:
  redis1:
    image: daocloud.io/library/redis:5.0.7
     #容器总是从新启动
    restart: always
    container_name: redis1
    environment:
      - TZ=Asia/Shanghai
    ports:
      - 7001:7001
      - 17001:17001
    volumes:
      - ./conf/redis1.conf:/usr/local/redis/redis.conf
    command: ["redis-server","/usr/local/redis/redis.conf"]
  redis2:
    image: daocloud.io/library/redis:5.0.7
     #容器总是从新启动
    restart: always
    container_name: redis2
    environment:
      - TZ=Asia/Shanghai
    ports:
      - 7002:7002
      - 17002:17002
    volumes:
      - ./conf/redis2.conf:/usr/local/redis/redis.conf
    command: ["redis-server","/usr/local/redis/redis.conf"]
  redis3:
    image: daocloud.io/library/redis:5.0.7
     #容器总是从新启动
    restart: always
    container_name: redis3
    environment:
      - TZ=Asia/Shanghai
    ports:
      - 7003:7003
      - 17003:17003
    volumes:
      - ./conf/redis3.conf:/usr/local/redis/redis.conf
    command: ["redis-server","/usr/local/redis/redis.conf"]
  redis4:
    image: daocloud.io/library/redis:5.0.7
     #容器总是从新启动
    restart: always
    container_name: redis4
    environment:
      - TZ=Asia/Shanghai
    ports:
      - 7004:7004
      - 17004:17004
    volumes:
      - ./conf/redis4.conf:/usr/local/redis/redis.conf
    command: ["redis-server","/usr/local/redis/redis.conf"]
  redis5:
    image: daocloud.io/library/redis:5.0.7
     #容器总是从新启动
    restart: always
    container_name: redis5
    environment:
      - TZ=Asia/Shanghai
    ports:
      - 7005:7005
      - 17005:17005
    volumes:
      - ./conf/redis5.conf:/usr/local/redis/redis.conf
    command: ["redis-server","/usr/local/redis/redis.conf"]
  redis6:
    image: daocloud.io/library/redis:5.0.7
     #容器总是从新启动
    restart: always
    container_name: redis6
    environment:
      - TZ=Asia/Shanghai
    ports:
      - 7006:7006
      - 17006:17006
    volumes:
      - ./conf/redis6.conf:/usr/local/redis/redis.conf
    command: ["redis-server","/usr/local/redis/redis.conf"]

```



```
#redis.conf
# 指定redis的端口号
port 7001
#开启redis集群
cluster-enabled yes
# 集群信息的文件
cluster-config-file nodes-7001.conf
# 集群的对外ip地址
cluster-announce-ip 192.168.1.10
# 集群的对外端口号
cluster-announce-port 7001
#集群的总线端口号
cluster-announce-bus-port 17001
```





> 启动6个Redis的节点。
>
> 随便跳转到一个容器内部，使用redis-cli管理集群，他会自动分配好主从节点以及hash槽


配置 redis 集群，共 6 台机器 ， 集群中 master 节点 有 1个副本，因此 会被配置为 3 个 master节点，每个节点一个副本
```shell
redis-cli --cluster create 192.168.1.10:7001 192.168.1.10:7002 192.168.1.10:7003 192.168.1.10:7004 192.168.1.10:7005 192.168.1.10:7006 --cluster-replicas 1
```



测试

使用redis-cli -h 192.168.102.11 -p 7001 连接指定一个redis节点，此时set key可能设置不进去，因为通过计算key应该在另外的节点。如果需要在客户端连接，但是set数据能跳转到其他节点set，连接命令需要加-c，如下

redis-cli -h 192.168.102.11 -p 7001 -c



