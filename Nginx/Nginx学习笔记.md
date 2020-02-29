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

  