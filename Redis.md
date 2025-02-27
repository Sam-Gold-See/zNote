# Redis

[Redis](https://www.redis.net.cn/) 是一个基于**内存**的**key-value**结构数据库

- 基于内存存储，读写性能高

- 适合存储热点数据

- 企业应用广泛

`redis.windows.conf`：Redis 配置文件

`redis-cli.exe`：Redis 客户端

`redis-server.exe`：Redis 服务端

## 启动

在命令行界面输入`redis-server.exe redis.windows.conf`启动 Redis 服务器

在新命令行界面输入`redis-cli.exe`启动 Redis 客户端

- 连接到 Redis 服务器：`redis-cli -h 127.0.0.1 -p 6379`

- 使用密码：`-a <yourpassword>`

## 数据类型

Redis 存储的是 key-value 结构的数据，其中 key 是字符串类型，value 有 5 中常用的数据类型：

- 字符串 string：普通字符串，Redis 中最简单的数据类型

- 哈希 hash：也叫散列，类似于 Java 中的 HashMap 结构

- 列表 list：按照插入顺序排序，可以有重复元素，类似于 Java 中的 LinkedList

- 集合 set：无序集合，没有重复元素，类似于 Java 中的 HashSet

- 有序集合 zset / sorted set：集合中每个元素关联一个分数（score），根据分数升序排序，没有重复元素

## 常用命令

### 字符串操作命令

- `SET key value`：设置指定 key 的值

- `GET key`：获取指定 key 的值

- `SETEX key seconds value`：设置指定 key 的值，并将 key 的过期时间设置为 seconds 秒

- `SETNX key value`：只有在 key 不存在时设置 key 的值

### 哈希操作命令

- `HSET key field value`：将哈希表 key 中的字段 field 的值设为 value

- `HGET key field`：获取存储在哈希表中的指定字段

- `HDEL key filed`：删除存储在哈希表中的指定字段

- `HKEYS key`：获取哈希表中所有字段

- `HVALS key`：获取哈希表中所有值

### 列表操作命令

- `LPUSH key value1 [value2...]`：将一个或多个值插入到列表头部

- `LRANGE key start stop`：获取列表指定范围内的元素

- `RPOP key`：移除并获取列表最后一个元素

- `LLEN key`：获取列表长度

### 集合操作命令

- `SADD key member1 [member2...]`：向集合添加一个或多个成员

- `SMEMBERS key`：返回集合中的所有成员

- `SCARD key`：获取集合的成员数

- `SINTER key1 [key2...]`：返回给定所有集合的交集

- `SUNION key1 [key2...]`：返回所有给定集合的并集

- `SREM key member1 [member2...]`：删除集合中一个或多个成员

### 有序集合操作命令

- `ZADD key score1 member1 [score2 member2...]`：向有序集合添加一个或多个成员

- `ZRANGE key start stop [WITHSCORES]`：通过索引区间返回有序集合中指定区间内的成员

- `ZINCRBY key increment member`：有序集合中对指定成员的分数加上增量 increment

- `ZREM key member1 [member2...]`：移除有序集合中的一个或多个成员

### 通用命令

Redis 的通用命令是部分数据类型的，都可以使用的命令：

- `KEYS pattern`：查找所有符合给定模式的 key

- `EXISTS key`：检查给定的 key 是否存在

- `TYPE key`：返回 key 所储存的值的类型

- `DEL key`：该命令用于在 key 存在时删除 key
