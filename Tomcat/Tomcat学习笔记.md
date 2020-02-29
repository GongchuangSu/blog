### tomcat用户管理权限

配置文件地址：`conf/tomcat-users.xml`

```xml
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <role rolename="manager-jmx"/>
  <role rolename="manager-status"/>
  <user username="tomcat" password="tomcat" roles="tomcat"/>
  <user username="both" password="tomcat" roles="tomcat,role1"/>
  <user username="role1" password="tomcat" roles="role1"/>
  <user username="root" password="root" roles="tomcat,role1,manager-gui,manager-script,manager-jmx,manager-status"/>
```

