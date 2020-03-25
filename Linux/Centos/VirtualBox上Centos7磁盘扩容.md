```
扇区-->物理分区-->物理卷(pv)-->卷组(vg)-->逻辑卷(lv)-->文件系统
```

- 增加虚拟机可用磁盘空间

  【管理】-【虚拟介质管理器】

  ![image-20200325110146146](assets/image-20200325110146146.png)

- 查看文件系统容量使用情况：`df -hl`

  ![image-20200325102252141](assets/image-20200325102252141.png)

- 查看现有磁盘和分区情况：`fdisk -l`

  ![image-20200325103655346](assets/image-20200325103655346.png)

- 给sdk磁盘新建分区：`fdisk /dev/sda`

  ![image-20200325104723478](assets/image-20200325104723478.png)

  ![image-20200325105213689](assets/image-20200325105213689.png)

- 格式化新建的分区(格式化前需要先重启,命令reboot)：`mkfs.xfs /dev/sda4`

  ![image-20200325105356206](assets/image-20200325105356206.png)

- 把新加的硬盘分区创建为物理卷

  查看命令：`pvdisplay`

  创建命令：`pvcreate /dev/sda4`

  ![image-20200325105722328](assets/image-20200325105722328.png)

- 将新建的物理卷(pv)添加到卷组(vg)

  查看命令：`vgdisplay`

  扩展命令：`vgextend centos /dev/sda4`

  ![image-20200325095948690](assets/image-20200325095948690.png)

  ![image-20200325100137850](assets/image-20200325100137850.png)

- 扩展逻辑卷(lv)

  查看命令：`lvdisplay`

  扩展命令：`lvextend -L +6G /dev/centos/root`

  ![image-20200325100818466](assets/image-20200325100818466.png)

  ![image-20200325101118075](assets/image-20200325101118075.png)

- 扩展文件系统

  查看命令：`df -hl`

  扩展命令：`xfs_growfs /dev/centos/root`

  ![image-20200325101404042](assets/image-20200325101404042.png)

  ![image-20200325101639173](assets/image-20200325101639173.png)

# 参考资料

- [VirtualBox上Centos7磁盘扩容](https://blog.csdn.net/haeydy/article/details/89447689)