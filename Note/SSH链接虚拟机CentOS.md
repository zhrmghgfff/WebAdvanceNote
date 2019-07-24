# SSH工具连接虚拟机中的CentOS7

## 确保CentOS7安装了  ifconfig

```
yum upgrade
yum install net-tools
```

## 确保CentOS7安装了  openssh-server

```
yum list installed | grep openssh-server
```

有输出就是已安装
如果未安装，安装命令：

```
yum install openssh-server
```

