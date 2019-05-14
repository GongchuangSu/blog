## 修改PATH环境变量

### 临时修改，关闭连接失效

```shell
export PATH=/usr/local/bin:$PATH
```

> 有效期限：临时改变，只能在当前的终端窗口中有效，当前窗口关闭后就会恢复原有的path配置
> 用户局限：仅对当前用户

### 永久修改当前用户

```shell
vim ~/.bashrc 
// 在最后一行添上：
export PATH=/usr/local/bin:$PATH
// 关闭保存，执行以下命令生效
source ~/.bashrc
```

> 有效期限：永久有效
> 用户局限：仅对当前用户

### 全局修改

```shell
vim /etc/profile
//在最后一行添上：
export PATH=/usr/local/bin:$PATH
// 关闭保存，执行以下命令生效
source ~/.bashrc
```

> 有效期限：永久有效
> 用户局限：对所有用户

