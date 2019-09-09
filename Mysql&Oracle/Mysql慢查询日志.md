## 前言

数据库日志记录了用户对数据库的各种操作以及数据库发生的各种事件，能帮助数据库管理员追踪、分析问题。

MySQL提供了以下几种数据库日志：

- 错误日志：默认开启，文件名称通常为hostname.err
- 二进制日志：也称为变更日志，主要用于记录修改数据或有可能引起数据改变的mysql语句，并且记录了语句发生时间、执行时长、操作的数据等
  - `log_bin`：定义二进制日志是否开启
- 查询日志：默认不开启，会记录用户的所有操作，包括增删改查
  - `general_log`：定义查询日志是否开启
  - `general_log_file`：定义查询日志的文件
- 慢查询日志：默认不开启

MySQL的慢查询日志是MySQL提供的一种日志记录，用来记录在MySQL中响应时间超过阈值（long_query_time，单位：秒）的SQL语句。

接下来，本文将介绍如何开启慢查询日志以及如何通过慢查询分析工具进行日志分析。

## 慢查询日志配置

### 开启慢查询日志

#### 修改my.cnf

在配置文件`my.cnf`（一般位于`/etc/mysql/my.cnf`）中的[mysqld]下添加如下参数：

```
[mysqld]
slow_query_log = 1
slow_query_log_file = /var/lib/mysql/slow_query.log
long_query_time = 1
log_queries_not_using_indexes = 0
```

说明：

- slow_query_log：表示是否开启慢查询
- slow_query_log_file：指定慢查询日志路径，若没有指定，则默认名字为hostname_slow.log
- long_query_time：查询时间阈值，表示查询时间超过该阈值才会被记录到日志文件中
- log_queries_not_using_indexes：表示记录那些没有使用索引的SQL语句

#### 重启MySQL服务

- 使用**service**启动

  ```shell
  service mysqld restart
  ```

- 使用**mysqld**脚本启动

  ```shell
  /etc/init.d/mysqld restart
  ```

- 若部署在docker环境下，可直接重启容器

  ```shell
  docker restart 容器ID
  ```

重启服务后便可看到/var/lib/mysql/slow_query.log文件

#### 查看参数

```mysql
mysql> show variables like '%slow_query%';
+---------------------+-------------------------------+
| Variable_name       | Value                         |
+---------------------+-------------------------------+
| slow_query_log      | ON                            |
| slow_query_log_file | /var/lib/mysql/slow_query.log |
+---------------------+-------------------------------+
2 rows in set (0.01 sec)

mysql> show variables like '%long_query%';
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 1.000000 |
+-----------------+----------+
1 row in set (0.01 sec)
```

#### 验证配置是否生效

- 制造慢查询

  ```mysql
  mysql> select sleep(1);
  +----------+
  | sleep(1) |
  +----------+
  |        0 |
  +----------+
  1 row in set (1.00 sec)
  ```

- 查看慢查询日志

  ```shell
  root@f6f1f09f85b0:/# more /var/lib/mysql/slow_query.log
  /usr/sbin/mysqld, Version: 8.0.16 (MySQL Community Server - GPL). started with:
  Tcp port: 0  Unix socket: /var/run/mysqld/mysqld.sock
  Time                 Id Command    Argument
  # Time: 2019-09-09T23:07:38.468693Z
  # User@Host: root[root] @ localhost []  Id:     8
  # Query_time: 1.000925  Lock_time: 0.000000 Rows_sent: 1  Rows_examined: 0
  SET timestamp=1568070457;
  select sleep(1);
  ```

  

## 慢查询日志分析

mysqldumpslow是MySQL自带的分析慢查询的工具。常见参数如下：

```shell
-s:排序方式，其后可带如下参数
	c:查询次数
	t:查询时间
	l:锁定时间
	r:返回记录
	ac:平均查询时间
	al:平均锁定时间
	ar:平均返回记录数
	at:平均查询时间
-t:top N查询
-g:正则表达式
```

#### 实例

- 获取查询次数Top5的SQL语句

  ```shell
  $ mysqldumpslow -s c -t 5 /var/lib/mysql/slow-query.log
  ```

- 获取查询时间Top5的SQL语句

  ```shell
  $ mysqldumpslow -s t -t 5 /var/lib/mysql/slow-query.log
  ```

- 获取查询时间Top5且含有`like`的SQl语句

  ```shell
  $ mysqldumpslow -s t -t 5 -g "like" /var/lib/mysql/slow-query.log
  ```

## 参考文献

- [如何开启MySQL慢查询日志](https://yq.aliyun.com/articles/603174)
- [MySQL慢查询分析mysqldumpslow](http://kimi.it/410.html)