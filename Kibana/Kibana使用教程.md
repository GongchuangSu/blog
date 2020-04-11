# 安装

## 下载安装kibana

```shell
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.6.1-linux-x86_64.tar.gz
sha1sum kibana-7.6.1-linux-x86_64.tar.gz
tar -zxf kibana-7.6.1-linux-x86_64.tar.gz
mv kibana-7.6.1-linux-x86_64 kibana
cd kibana
```

# 使用教程

## 开启安全设置

- 从6.8和7.1.0开始免费提供基本安全配置

- Elasticsearch

  配置文件elasticsearch.yml

  ```shell
  xpack.security.enabled: true
  xpack.security.transport.ssl.enabled: true
  ```

- 设置缺省用户密码

  `./bin/elasticsearch-setup-passwords interactive`

- Kibana设置

  配置文件kibina.yml

  ```shell
  elasticsearch.name: "kibana"
  elasticsearch.password: "elastic"
  ```

  

# 参考资料

- [Kibana 用户手册](https://www.elastic.co/guide/cn/kibana/current/index.html)
- [elastic社区](https://elasticsearch.cn/)
- [k8s日志采集系统说明]([http://faq.egova.com.cn:7777/projects/redmine/wiki/K8s%E6%97%A5%E5%BF%97%E9%87%87%E9%9B%86%E7%B3%BB%E7%BB%9F%E8%AF%B4%E6%98%8E](http://faq.egova.com.cn:7777/projects/redmine/wiki/K8s日志采集系统说明))

