# Redis基础数据结构

Redis 有 5 种基础数据结构，分别为：string (字符串)、list (列表)、set (集合)、hash (哈希) 和 zset (有序集合)。

![image-20200225075052550](assets/image-20200225075052550.png)

## string（字符串）

> 字符串 string 是Redis 最简单的数据结构。Redis 所有的数据结构都是以唯一的 key 字符串作为名称，然后通过这个唯一 key 值来获取相应的 value 数据。不同类型的数据结构的差异就在于 value 的结构不一样。字符串结构使用非常广泛，一个常见的用途就是缓存用户信息。我们将用户信息结构体使用 JSON 序列化成字符串，然后将序列化后的字符串塞进 Redis 来缓存。同样，取用户信息会经过一次反序列化的过程。

### 键值对

```shell
127.0.0.1:6379> set name redis
OK
127.0.0.1:6379> get name
"redis"
127.0.0.1:6379> del name
(integer) 1
127.0.0.1:6379> get name
(nil)
```

### 批量键值对

可以批量对多个字符串进行读写，节省网络耗时开销

```shell
127.0.0.1:6379> mset name1 Tom name2 Jack
OK
127.0.0.1:6379> mget name1 name2 name3
1) "Tom"
2) "Jack"
3) (nil)
```

### 过期和 set 命令扩展

可以对 key 设置过期时间，到点自动删除，这个功能常用来控制缓存的失效时间

- 通过`set`和`expire`设置过期

```shell
127.0.0.1:6379> set name Tom
OK
127.0.0.1:6379> get name
"Tom"
127.0.0.1:6379> expire name 5 # 设置 name 5秒后过期
(integer) 1
... # 等待5秒
127.0.0.1:6379> get name
(nil)
```

- **SETEX key seconds value**：设置过期时间

> 将键 `key` 的值设置为 `value` ， 并将键 `key` 的生存时间设置为 `seconds` 秒钟。
>
> 如果键 `key` 已经存在， 那么 `SETEX` 命令将覆盖已有的值。

```shell
127.0.0.1:6379> setex name 60 Tom # 设置 name 60秒后过期，等价于set + expire
OK
127.0.0.1:6379> get name
"Tom"
127.0.0.1:6379> ttl name # 剩余时间
(integer) 51
... # 等待N秒
127.0.0.1:6379> get name
(nil)
```

- **SETNX key value**

> 只在键 `key` 不存在的情况下， 将键 `key` 的值设置为 `value` 。
>
> 若键 `key` 已经存在， 则 `SETNX` 命令不做任何动作。
>
> `SETNX` 是『SET if Not eXists』(如果不存在，则 SET)的简写。

```shell
127.0.0.1:6379> setnx name Tom # 如果name不存在，则执行set命令创建
(integer) 1
127.0.0.1:6379> get name
"Tom"
127.0.0.1:6379> setnx name Jack # 如果name存在，则不做任何操作
(integer) 0
127.0.0.1:6379> get name
"Tom"
```

### 原子计数

- **INCR key**

> 为键 `key` 储存的数字值加上一。
>
> 如果键 `key` 不存在， 那么它的值会先被初始化为 `0` ， 然后再执行 `INCR` 命令。
>
> 如果键 `key` 储存的值不能被解释为数字， 那么 `INCR` 命令将返回一个错误。
>
> 本操作的值限制在 64 位(bit)有符号数字表示之内。

- **INCRBY key increment**

> 为键 `key` 储存的数字值加上增量 `increment` 。
>
> 如果键 `key` 不存在， 那么键 `key` 的值会先被初始化为 `0` ， 然后再执行 `INCRBY` 命令。
>
> 如果键 `key` 储存的值不能被解释为数字， 那么 `INCRBY` 命令将返回一个错误。
>
> 本操作的值限制在 64 位(bit)有符号数字表示之内。

```shell
127.0.0.1:6379> incr count
(integer) 1
127.0.0.1:6379> incr count
(integer) 2
127.0.0.1:6379> incr count
(integer) 3
127.0.0.1:6379> get count
"3"
127.0.0.1:6379> incrby count 5
(integer) 8
127.0.0.1:6379> get count
"8"

127.0.0.1:6379> set count 9223372036854775807 # Long.MAX
OK
127.0.0.1:6379> incr count
(error) ERR increment or decrement would overflow

```

同理，对应自减命令`DECR key`和`DECRBY key decrement`



##  list（列表）

> Redis 的列表相当于 Java 语言里面的 LinkedList，注意它是链表而不是数组。这意味着 list 的插入和删除操作非常快，时间复杂度为 O(1)，但是索引定位很慢，时间复杂度为 O(n)。 当列表弹出了最后一个元素之后，该数据结构自动被删除，内存被回收。
>
> Redis 的列表结构常用来做异步队列使用。将需要延后处理的任务结构体序列化成字符串塞进 Redis 的列表，另一个线程从这个列表中轮询数据进行处理。

### 右边进左边出（先进先出）：队列

```shell
127.0.0.1:6379> rpush books java python golang
(integer) 3
127.0.0.1:6379> llen books
(integer) 3
127.0.0.1:6379> lpop books
"java"
127.0.0.1:6379> lpop books
"python"
127.0.0.1:6379> lpop books
"golang"
127.0.0.1:6379> lpop books
(nil)
```

### 右边进右边出（先进后出）：栈

```shell
127.0.0.1:6379> rpush books java python golang
(integer) 3
127.0.0.1:6379> rpop books
"golang"
127.0.0.1:6379> rpop books
"python"
127.0.0.1:6379> rpop books
"java"
127.0.0.1:6379> rpop books
(nil)
```

## hash（哈希）

> Redis 的字典相当于 Java 语言里面的 HashMap，它是无序字典。内部实现结构上同 Java 的 HashMap 也是一致的，同样的数组 + 链表二维结构。第一维 hash 的数组位置碰撞时，就会将碰撞的元素使用链表串接起来。
>
> hash 结构也可以用来存储用户信息，不同于字符串一次性需要全部序列化整个对象，hash 可以对 用户结构中的每个字段单独存储。这样当我们需要获取用户信息时可以进行部分获取。而以整个字符串的形式去保存用户信息的话就只能一次性全部读取，这样就会比较浪费网络流量。 hash 也有缺点，hash 结构的存储消耗要高于单个字符串，到底该使用 hash 还是字符串，需要根据实际情况再三权衡。

```shell
127.0.0.1:6379> hset books java "think in java" # 设置值
(integer) 1
127.0.0.1:6379> hset books go "go in action"
(integer) 1
127.0.0.1:6379> hset books python "python cookbook"
(integer) 1

127.0.0.1:6379> hgetall books # 获取 books 下的所有key-value
1) "java"
2) "think in java"
3) "go"
4) "go in action"
5) "python"
6) "python cookbook"

127.0.0.1:6379> hlen books # 获取 books 下的键值对个数
(integer) 3
127.0.0.1:6379> hget books java
"think in java"
127.0.0.1:6379> hset books java "Efficient java"
(integer) 0
127.0.0.1:6379> hget books java
"Efficient java"

127.0.0.1:6379> hmset books java "think in java" go "go in action" python "python cookbook" # 批量set
OK
```

## set（集合）

> Redis 的集合相当于 Java 语言里面的 HashSet，它内部的键值对是无序的唯一的。它的内部实现相当于一个特殊的字典，字典中所有的 value 都是一个值NULL。 当集合中最后一个元素移除之后，数据结构自动删除，内存被回收。

```shell
127.0.0.1:6379> sadd books python
(integer) 1
127.0.0.1:6379> sadd books python # 若已存在，则返回0
(integer) 0
127.0.0.1:6379> sadd books java golang # 可批量添加
(integer) 2
127.0.0.1:6379> smembers books # 注意顺序，无序
1) "golang"
2) "python"
3) "java"
127.0.0.1:6379> sismember books java # 查询某个value是否存在，相当于contain(o)
(integer) 1
127.0.0.1:6379> sismember books ruby
(integer) 0
127.0.0.1:6379> scard books # 获取长度，相当于count()
(integer) 3
127.0.0.1:6379> spop books # 弹出一个，无顺序
"golang"
127.0.0.1:6379> spop books
"python"
127.0.0.1:6379> spop books
"java"
```

## zset（有序集合）

> zset 似于 Java 的 SortedSet 和 HashMap 的结合体，一方面它是一个 set，保证了内部 value 的唯一性，另一方面它可以给每个 value 赋予一个 score，代表这个 value 的排序权重。

zset 可以用来存粉丝列表，value 值是粉丝的用户 ID，score 是关注时间。我们可以对粉丝列表按关注时间进行排序。 

zset 还可以用来存储学生的成绩，value 值是学生的 ID，score 是他的考试成绩。我们可以对成绩按分数进行排序就可以得到他的名次。

```shell
127.0.0.1:6379> zadd books 9.0 java # 添加元素
(integer) 1
127.0.0.1:6379> zadd books 8.9 go
(integer) 1
127.0.0.1:6379> zadd books 8.5 python
(integer) 1
127.0.0.1:6379> zrange books 0 -1 # 按score顺序列出，参数为score范围
1) "python"
2) "go"
3) "java"
127.0.0.1:6379> zrevrange books 0 -1 # 按score逆序列出，参数score范围
1) "java"
2) "go"
3) "python"
127.0.0.1:6379> zcard books # 获取长度，相当于count()
(integer) 3
127.0.0.1:6379> zscore books java # 获取指定value的score
"9"
127.0.0.1:6379> zrank books java # 获取排名信息
(integer) 2
127.0.0.1:6379> zrank books go
(integer) 1
127.0.0.1:6379> zrank books python
(integer) 0
127.0.0.1:6379> zrangebyscore books 0 9 # 获取区间范围内value
1) "python"
2) "go"
3) "java"
127.0.0.1:6379> zrangebyscore books 0 8.9
1) "python"
2) "go"
127.0.0.1:6379> zrangebyscore books -inf 8.9 withscores # inf表示infinite
1) "python"
2) "8.5"
3) "go"
4) "8.9000000000000004"
127.0.0.1:6379> zrem books java # 删除指定value
(integer) 1
127.0.0.1:6379> zrange books 0 -1
1) "python"
2) "go"

```

## 其他高级命令
### keys

> **全量遍历键**，用来列出所有满足特定正则字符串规则的key，当redis数据量比较大时，性能比较差，要避免使用**全量遍历键**，用来列出所有满足特定正则字符串规则的key，当redis数据量比较大时，性能比较差，要避免使用

```shell
127.0.0.1:6379> keys *
1) "books"
2) "names"
```

### scan

> **scan：渐进式遍历键，**scan 参数提供了三个参数，第一个是 cursor 整数值，第二个是 key 的正则模式，第三个是遍历的 limit hint。第一次遍历时，cursor 值为 0，然后将返回结果中第一个整数值作为下一次遍历的 cursor。一直遍历到返回的 cursor 值为 0 时结束。

```shell
127.0.0.1:6379> keys name*
 1) "name1"
 2) "name9"
 3) "name6"
 4) "name8"
 5) "name10"
 6) "name5"
 7) "name3"
 8) "name7"
 9) "names"
10) "name2"
11) "name4"
127.0.0.1:6379> scan 0 match name* count 5
1) "10"
2) 1) "name1"
   2) "name8"
   3) "names"
   4) "name9"
127.0.0.1:6379> scan 10 match name* count 5
1) "15"
2) 1) "name10"
   2) "name3"
   3) "name7"
   4) "name6"
   5) "name5"
127.0.0.1:6379> scan 15 match name* count 5
1) "0"
2) 1) "name2"
   2) "name4"
```

### info

> 查看redis服务运行信息，分为 9 大块，每个块都有非常多的参数，这 9 个块分别是: 
>
> Server：服务器运行的环境参数 
>
> Clients：客户端相关信息 
>
> Memory：服务器运行内存统计数据 
>
> Persistence：持久化信息 
>
> Stats：通用统计数据 
>
> Replication：主从复制相关信息 
>
> CPU CPU：使用情况 
>
> Cluster：集群信息 
>
> KeySpace：键值对统计数量信息

```shell
127.0.0.1:6379> info
# Server
redis_version:3.2.12		# Redis版本
redis_git_sha1:00000000	# git上版本
redis_git_dirty:0				# git的代码是否修改
redis_build_id:7897e7d0e13773f				# 编译时ID
redis_mode:standalone									# redis运行模式
os:Linux 3.10.0-862.el7.x86_64 x86_64	# 所运行操作系统内核
arch_bits:64						# 64架构
multiplexing_api:epoll	# Redis基于epoll模型
gcc_version:4.8.5				# gcc版本
process_id:10855				# 进程PID
run_id:d7890df4417cf1238711510a27adc3e285322127	# Redis的随机标识符（用于sentinel和集群）
tcp_port:6379						# Redis端口
uptime_in_seconds:1427	# Redis运行时长的秒数
uptime_in_days:0				# Redis运行的天数
hz:10										# 时间事件，单位为赫兹，每10毫秒循环一次
lru_clock:5838964				# 以分钟为单位的自增时钟，用于LRU管理maxmemory-policy内存回收策略
executable:/usr/bin/redis-server	# 可执行文件位置
config_file:/etc/redis.conf				# 配置文件位置

# Clients
connected_clients:1						# 已连接客户端的数量（不包括通过从属服务器连接的客户端）
client_longest_output_list:0	# 所有客户端连接中，最长的输出列表，Redis输出缓存峰值
client_biggest_input_buf:0		# 所有客户端连接中，最大输入缓存，Redis输入缓存峰值
blocked_clients:0							# 正在等待阻塞命令（BLPOP、BRPOP、BRPOPLPUSH）的客户端的数量

# Memory
used_memory:813280								# 由Redis分配的内存的总量，字节数
used_memory_human:794.22K					# 同上，单位K
used_memory_rss:6053888						# Redis进程从OS角度分配的物理内存，如key被删除后，malloc不一定把内存归还给OS，但可以Redis进程复用，代表Redis使用的总内存，字节数
used_memory_rss_human:5.77M				# 同上，单位M
used_memory_peak:813416						# Redis使用内存的峰值，字节数
used_memory_peak_human:794.35K		# 同上，单位K
total_system_memory:1039880192
total_system_memory_human:991.71M
used_memory_lua:37888							# lua引擎使用的内存总量，字节数
used_memory_lua_human:37.00K			# 同上，单位K
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
mem_fragmentation_ratio:7.44			
mem_allocator:jemalloc-3.6.0			# Redis内存管理器

# Persistence
loading:0	# 标识位，是否在载入数据文件，0代表没有，1代表正在载入
rdb_changes_since_last_save:0		# 从最近一次dump快照后，未被dump的变更次数	
rdb_bgsave_in_progress:0				# 标识位，记录当前是否在创建RDB快照
rdb_last_save_time:1582897043		# 最近一次创建RDB快照文件的Unix时间戳
rdb_last_bgsave_status:ok				# 标识位，记录最近一次bgsave操作是否创建成功
rdb_last_bgsave_time_sec:0			# 最近一次bgsave操作耗时秒数
rdb_current_bgsave_time_sec:-1	# 当前bgsave执行耗时秒数（-1表示还未执行）
aof_enabled:1										# appendonly是否开启
aof_rewrite_in_progress:0				# AOF重写是否正在进行
aof_rewrite_scheduled:0					# AOF重写是否被RDB save操作阻塞等待
aof_last_rewrite_time_sec:-1		# 最近一次AOF重写操作耗时
aof_current_rewrite_time_sec:-1	# 当前AOF重写持续的耗时
aof_last_bgrewrite_status:ok		# 最近一次AOF重写操作是否成功
aof_last_write_status:ok				# 最近一次AOF写入操作是否成功
aof_current_size:525						# AOF文件目前的大小，字节数
aof_base_size:0									# 服务器启动时或者AOF重写最近一次执行之后，AOF文件的大小
aof_pending_rewrite:0						# 是否有AOF重写操作在等待RDB文件创建完毕之后执行
aof_buffer_length:0							# AOF缓冲区的大小
aof_rewrite_buffer_length:0			# AOF重写缓冲区的大小
aof_pending_bio_fsync:0					# 后台I/O队列里面，等待执行的fsync调用数量
aof_delayed_fsync:0							# 被延迟的fsync调用数量

# Stats
total_connections_received:1		# 服务器已接受的连接请求数量
total_commands_processed:28			# 服务器已执行的命令数量
instantaneous_ops_per_sec:0			# 服务器每秒执行的命令数量
total_net_input_bytes:1045			# Redis每秒网络输入的字节数
total_net_output_bytes:10583		# Redis每秒网络输出的字节数
instantaneous_input_kbps:0.00		# 瞬间的Redis输入网络流量（kbps）
instantaneous_output_kbps:0.00  # 瞬间的Redis输出网络流量（kbps）
rejected_connections:0					# 因连接数达到maxclients上限后，被拒绝的连接个数
sync_full:0					# 累计master full sync的次数；如果值比较大，说明常常出现全量复制，就得分析原因或调整repl-backlog-size
sync_partial_ok:0		# 累计master psync成功的次数
sync_partial_err:0	# 累计master psync失败的次数
expired_keys:0			# 因过期而被自动删除的数据库键数量
evicted_keys:0			# 因内存used_memory达到maxmemory后，每秒被驱逐的key个数
keyspace_hits:1			# 查找键命中的次数
keyspace_misses:0		# 查找键未命中的次数
pubsub_channels:0		# 目前被订阅的频道数量
pubsub_patterns:0		# 目前被订阅的模式数量
latest_fork_usec:344			# 最近一次fork操作的耗时的微秒数（BGREWRITEAOF、BGSAVE、SYNC等都会触发fork）
migrate_cached_sockets:0

# Replication
role:master	# 当前Redis的主从状态
connected_slaves:0			# 下面有几个slave
master_repl_offset:0		# master复制的偏移量
repl_backlog_active:0							# 标识位，master是否开启了repl_backlog
repl_backlog_size:1048576					# repl_backlog的长度，网络环境不稳定的，建议调整大些
repl_backlog_first_byte_offset:0	# repl_backlog中首字节的复制偏移位
repl_backlog_histlen:0	# repl_backlog当前使用的字节数

# CPU
used_cpu_sys:0.73						# Redis进程消耗的sys cpu
used_cpu_user:0.25					# Redis进程消耗的user cpu
used_cpu_sys_children:0.00	# 后台进程耗费的系统CPU
used_cpu_user_children:0.00	# 后台进程耗费的用户CPU

# Cluster
cluster_enabled:0 # 是否开启集群模式

# Keyspace
db0:keys=12,expires=0,avg_ttl=0	# key的总数,过期的key的数量,平均key过期的时间
```




# 常见知识点

## 为什么要用redis而不用map做缓存?

- Redis可以用几十G内存来做缓存，Map 不行，一般 JVM 也就分几个 G 数据就够大了
- Redis的缓存可以持久化，Map是内存对象，其生命周期随着jvm的销毁而结束
- Redis可以实现分布式的缓存，Map存在于其各自的实例中，不具有一致性
- Redis可以处理每秒百万级的并发，是专业的缓存服务，Map只是一个普通对象
- Redis缓存有过期机制，Map本身无此功能
- Redis有丰富的API，Map就简单太多



### 登陆验证

```shell
redis-cli -h 127.0.0.1 -p 6379 -a myPassword
```

