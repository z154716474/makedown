## Redis

​	Redis命令参考  http://doc.redisfans.com

​	Redis是高性能的，因为数据是保存在内存里的，key-value的非关系数据库(键不能重复)，不支持sql语法，有自己的语法

- **非关系数据库有**：Redis，MongoDB，Hbase-hadoop，Cassandra hadoop

- **与关系型数据库的比较**：sql数据库适合于关系复杂的数据场景，关系型数据库支持**事务**(要么一起成功要么都不成功，保证数据的一致性)，其实nosql也支持事务，但是有所不同。

- **部署/备份**：Redis数据备份，部署到其他地方，即master-slave模式的数据备份。

- **特性**：原子性的操作，Redis还支持publish/subscribe，通知，key过期，等特性。

- **数据结构**：Redis不仅支持key-value，还支持list，set，zset，hash，等数据结构的存储

  ![IMG_424F15C6A61E-1](/Users/samdi/Documents/Redis数据库/IMG_424F15C6A61E-1.jpeg)

### 1.1安装

下载 https://www.redis.net.cn/download/

解压到usr/local下cd到文件夹执行`make test`再执行`make install`安装完成

**配置文件是usr/local/redis/redis.conf**

开启服务`redis-server` 

开启客户端`redis-cli`连接服务端



### 1.2核心配置选项

- 绑定IP：如果需要远程访问，可将此行注释，或者绑定一个真实的IP

  > ​	bind 127.0.0.1

- 默认**端口6379**：

  > ​    port 6379

- 是否以守护进程的方式运行：
  - 如果以守护进程的方式运行，则不会在命令行阻塞，类似服务，设置为yes
  - 如果以非守护进程方式运行，则当前终端会被阻塞，设置为no

> ​	daemonize  yes

- 数据文件类型：

> ​	dbfilename dump.rdb

- 数据人间存储路径：

> ​	dir /....../redis

- 日志类型：

> ​	logfile "/.../redis-server.log"

- **数据库，默认有16个**：

> ​	database 16

- 主从复制，类似于双机备份：

> ​	slaveof

### 1.3 数据库操作指令

#### string

- **select 0** 切换数据库，切到0号库，默认有16个库，15号为最后一个
- 写入**set key value**
- 读取**get key**，获取value值
- **redis-check-aof** 这是AOF文件修复工具
- **redis-check-rdb** 这是RDB文件检索工具 
- **setex key time value**设置过期时间  （`-2`过期，`-1`无时效）
- **ttl key**查看过期时间
- **keys *** 查看当前号库的所有数据（可用正则）
- **mset key value .....**写入多个
- **mget key value....** 读取多个

#### 键命令

- **exists key**  判断key是否存在，存在返回1，不存在返回0 
- **type key** 查看value的类型
- **del key key ....**  删除(可删除多个)

#### hash

- **hset key key value** 设置一个大key和一个小key加上value
- **hget key key** 获取大key和小key返回value
- **hdel key key** 删除小key
- 多个操作hmset，hmget，hmdel

#### list

操作方式和Python的list类似，**大key相当于列表名**

- **lpush key index value** 左添加（用在浏览记录功能上）
- **rpush key index value** 右添加
- **lrange key start stop** 获取
  - start 和 stop 为下表索引 可以取-1（和Python的list取值一样）
- **lrem key count value** 删除指定元素
  - 将列表中前count次出现的值为value的元素移除
  - count >0 从头往尾移除
  - count <0 从尾往头移除
  - count =0 移除所有

#### set集合类型

特点：无序，不重复，没有修改操作

- **sadd key member** 添加成员
- **smembers key** 查看
- **srem key member** 移除

#### zset有序集合

特点：有序，不重复，每个元素都会关联一个double类型的score(权重)

- **zadd key score member**  添加 (可以以时间戳作为权重)
- **zrange key start stop** 查询
- **zrem key member** 移除

### 1.4 Redis与Python交互

```
pip3 install redis
```

```python
from redis import Redis
r = Redis(host='localhost',port='6379',db=0)
r.set('foo','bar')
....
```

