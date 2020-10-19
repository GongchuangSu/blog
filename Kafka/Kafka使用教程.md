### 查询topic列表

```shell
bash-4.4# kafka-topics.sh --list --zookeeper 192.168.213.135:2181
__consumer_offsets
egova-wf-publish-version
springCloudBus
```

###查看topic详情

```shell
bash-4.4# kafka-topics.sh --zookeeper 192.168.213.135:2181 --topic egova-wf-publish-version --describe
Topic: egova-wf-publish-version	PartitionCount: 1	ReplicationFactor: 1	Configs:
	Topic: egova-wf-publish-version	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
```

### 模拟生产者生产消息

```shell
bash-4.4# kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic egova-wf-publish-version
>hello
```

###模拟消费者接收消息

```shell
bash-4.4# kafka-console-consumer.sh --bootstrap-server 192.168.213.145:9092 --topic egova-wf-publish-version --from-beginning
hello
```

### 查看消费者组

```shell
bash-4.4# kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
console-consumer-54669
console-consumer-72755
```

### 查看消费组内消费情况

```shell
bash-4.4# kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group console-consumer-72755 --describe

GROUP                  TOPIC                    PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                                            HOST            CLIENT-ID
console-consumer-72755 egova-wf-publish-version 0          -               6               -               consumer-console-consumer-72755-1-4e2259bf-7b94-4de9-825e-62a553969c59 /172.18.0.1     consumer-console-consumer-72755-1
```



