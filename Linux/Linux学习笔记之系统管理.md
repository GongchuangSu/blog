## su命令

`su`命令用于切换当前用户身份到其他用户身份，变更时须输入所要变更的用户帐号与密码

```shell
## root用户切换到普通用户
[root@localhost ~]# su - test
## 普通用户切换到root用户，需输入密码
[test@localhost ~]$ su - root
```

