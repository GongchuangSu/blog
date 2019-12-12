```mysql
show binary logs;
show variables like '%log%';
```



- mysqlbinlog

  ```shell
  mysqlbinlog --no-defaults --base64-output=decode-rows -v --start-datetime='2019-11-13 00:00:00' --stop-datetime='2019-11-14 00:00:00' binlog.000049 > 20191114.sql
  ```

  ```shell
  cat 20191114.sql| grep -C 10 "DELETE FROM \`cgdb\`.\`tc_wf_act_def\`" > delete.sql
  ```

  

