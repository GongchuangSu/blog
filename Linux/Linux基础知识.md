# 目录和文件系统
- /：根目录
- /bin：重要的二进制(binary)应用程序
- /sbin：重要的系统二进制 (system binaries) 文件
- /usr/bin、/usr/sbin：系统预装的其他命令
- /boot：启动(boot)配置文件
- /dev：设备(device)文件
- /etc：配置文件、启动脚本等(etc)
- /home：本地用户主(home)目录
- /home/username：普通用户的家目录
- /lib：系统库(libraries)文件
- /lost+found：在根(/)目录下提供一个遗失+查找(lost+found)系统
- /media：挂载可移动介质(media)，诸如CD、数码相机等
- /mnt：挂载(mounted)文件系统
- /opt：提供一个供选择的(optional)应用程序安装目录
- /proc：特殊的动态目录，用以维护系统信息和状态，包括当前运行中进程 (processes) 信息。
- /root：root (root) 用户主文件夹，读作“slash-root”
- /sys：系统 (system) 文件
- /tmp：临时(temporary)文件
- /usr：包含绝大部分所有用户(users)都能访问的应用程序和文件
- /var：经常变化的(variable)文件，诸如日志或数据库等

# 命令提示符
## `[root@localhost ~]#`
其中，
- root：当前登陆用户
- localhost：主机名
- `~`：当前所在目录（家目录）
- `#`：超级用户的提示符（普通用户提示符是$）

## 命令格式
**命令 [选项] [参数]**
> 注意：
> 1.个别命令使用不遵循此格式。2.当有多个选项时，可以写在一。3.简化选项与完整选项-a等于--all