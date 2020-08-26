## catalina.out和catalina.log区别

- catalina.{yyyy-MM-dd}.out即标准输出和标准出错，所有输出到这两个位置的都会进入catalina.out，这里包含tomcat运行自己输出的日志以及应用里向console输出的日志。

- catalina.{yyyy-MM-dd}.log是tomcat自己运行的一些日志

- localhost.{yyyy-MM-dd}.log主要是应用初始化(listener, filter, servlet)未处理的异常最后被tomcat捕获而输出的日志，而这些未处理异常最终会导致应用无法启动
- localhost_access_log.{yyyy-MM-dd}.log是tomcat访问日志记录

