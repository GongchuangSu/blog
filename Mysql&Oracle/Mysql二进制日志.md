```mysql
show binary logs;
show variables like '%log%';
```

开启bin_log

```shell
#第一种方式
#开启binlog日志
log_bin=ON
#binlog日志的基本文件名
log_bin_basename=/var/lib/mysql/mysql-bin
#binlog文件的索引文件，管理所有binlog文件
log_bin_index=/var/lib/mysql/mysql-bin.index
#第二种方式，等同于上面三行
log-bin=/var/lib/mysql/mysql-bin
```



- mysqlbinlog

  ```shell
  mysqlbinlog --no-defaults --base64-output=decode-rows -v --start-datetime='2019-11-13 00:00:00' --stop-datetime='2019-11-14 00:00:00' binlog.000049 > 20191114.sql
  ```

  ```shell
  cat 20191114.sql| grep -C 10 "DELETE FROM \`cgdb\`.\`tc_wf_act_def\`" > delete.sql
  ```

  

