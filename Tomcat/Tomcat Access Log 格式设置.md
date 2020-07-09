配置文件位置: `conf/server.xml`

默认配置：

```xml
<Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
       prefix="localhost_access_log." suffix=".txt"
       pattern="%h %l %u %t &quot;%r&quot; %s %b %D" />
```

| 名称 | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| %a   | Remote IP address                                            |
| %A   | Local IP address                                             |
| %b   | Bytes sent, excluding HTTP headers, or ‘-‘ if zero           |
| %B   | Bytes sent, excluding HTTP headers                           |
| %h   | Remote host name (or IP address if enableLookups for the connector is false) |
| %H   | Request protocol                                             |
| %l   | Remote logical username from identd (always returns ‘-‘)     |
| %m   | Request method (GET, POST, etc.)                             |
| %p   | Local port on which this request was received                |
| %q   | Query string (prepended with a ‘?’ if it exists)             |
| %r   | First line of the request (method and request URI)           |
| %s   | HTTP status code of the response                             |
| %S   | User session ID                                              |
| %t   | Date and time, in Common Log Format                          |
| %u   | Remote user that was authenticated (if any), else ‘-‘        |
| %U   | Requested URL path                                           |
| %v   | Local server name                                            |
| %D   | Time taken to process the request, in millis                 |
| %T   | Time taken to process the request, in seconds                |
| %F   | Time taken to commit the response, in millis                 |
| %I   | Current request thread name (can compare later with stacktraces) |

默认的配置打出来的access日志如下：

|           |                  |             |                              |                        |        |                      |                |
| --------- | ---------------- | ----------- | ---------------------------- | ---------------------- | ------ | -------------------- | -------------- |
| 127.0.0.1 | -                | -           | [07/Oct/2016:22:31:56 +0800] | “GET /dubbo/ HTTP/1.1” | 404    | 963                  | 2              |
| 远程IP    | logical username | remote user | 时间和日期                   | http请求的第一行       | 状态码 | 除去http头的发送大小 | 请求时间，毫秒 |