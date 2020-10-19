## Mac上使用nginx

- 安装nginx

  ```shell
  brew install nginx
  ```

- 运行nginx

  ```shell
  nginx
  ```

- 重新加载nginx

  ```shell
  nginx -s reload
  ```

- 查看nginx配置文件路径

  ```shell
  nginx -t
  ```
  
- 默认文件路径

  ```shell
  # 配置文件路径
  /usr/local/etc/nginx
  # 日志文件路径
  /usr/local/var/log/nginx
  ```

- 启动和关闭nginx服务

  ```shell
  brew services start nginx
  brew services stop nginx
  ```

  

## 常用场景配置

### 根据请求参数值转发到不同地址

```nginx
# 根据请求参数进行转发
location ~* /jfive/egevent/send {
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $remote_addr;
  proxy_set_header Host $http_host;

  if ( $query_string ~ "humanID=\b(100433|100422)\b(.*)$" ) {
    proxy_pass http://huawei-j5-rc;
    break;
  }

  if ( $query_string ~ "humanID=890(.*)$" ) {
    proxy_pass http://huawei-j5-demo-rc;
    break;
  }

  return 403;
}

upstream huawei-j5-rc{
  server 1.119.137.18:35005;
}

upstream huawei-j5-demo-rc{
  server 1.119.137.19:35005;
}
```

