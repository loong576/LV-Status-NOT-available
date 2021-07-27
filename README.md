**操作系统版本**：CentOS Linux release 7.3.1611 (Core)

### 1.背景

有台服务器经过重新fdisk格盘后新建vg、lv和文件系统，但是重启后lv状态不正常，文件系统无法正常挂载。

### 2.现象

![image-20210726162843579](https://i.loli.net/2021/07/26/dzikSvCMX4UrTLG.png)

lv状态：“LV Status              NOT available”

### 3.问题汇总

> - 1.文件系统无法挂载；
> - 2.lv状态不正常；
> - 3.自启动脚本无法正常运行；

### 4.问题解决步骤

> - 1.新建服务lvmmount；
> - 2.注释/etc/fstab文件的挂载命令；
> - 3.调整自启动脚本；

### 5.lvmmount脚本

![image-20210726164209098](https://i.loli.net/2021/07/26/A5eMFglQJI2NUcG.png)

在/etc/init.d下新建服务lvmmount，建完后需要设置开机自启动

```bash
[root@xxx init.d]# systemctl  enable lvmmount
lvmmount.service is not a native service, redirecting to /sbin/chkconfig.
Executing /sbin/chkconfig lvmmount on
```

### 6.注释/etc/fstab

![image-20210726164302807](https://i.loli.net/2021/07/26/fTApGnB1KvR6DQE.png)

注释/etc/fstab，将挂载文件系统命令注释

### 7.调整自启动脚本

![image-20210726164357320](https://i.loli.net/2021/07/26/HyI9gdzP8EVlTLo.png)

在应用启动前新增睡眠时间5秒钟

### 8.最终效果

![image-20210726163600200](https://i.loli.net/2021/07/26/76oia5KLyHWXUhO.png)

文件系统挂载正常，lv状态正常，应用启动正常，问题解决。

