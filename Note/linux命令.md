[TOC]

#Linux命令

## 文件权限

* 修改单个文件（夹）

```
chmod  权限     文件（夹）
chown  名:组    文件（夹）
```

* 修改文件夹及子文件夹所有文件

```
chmod  -R  权限     文件夹
chown  -R  名:组    文件夹
```

## 文件操作

## SSH文件操作
远程获取文件

```
scp -P 2222 root@www.vpser.net:/root/lnmp0.4.tar.gz /home/lnmp0.4.tar.gz

上端口大写P 为参数，2222 表示更改SSH端口后的端口，如果没有更改SSH端口可以不用添加该参数
```

远程获取目录

```
scp -P 2222 -r root@www.vpser.net:/root/lnmp0.4/ /home/lnmp0.4/
```

上传文件

```
scp -P 2222 /home/lnmp0.4.tar.gz root@www.vpser.net:/root/lnmp0.4.tar.gz
```

上传目录

```
scp -P 2222 -r /home/lnmp0.4/ root@www.vpser.net:/root/lnmp0.4/
```

其他参数

```
-v 和大多数 linux 命令中的 -v 意思一样 , 用来显示进度 . 可以用来查看连接 , 认证 , 或是配置错误 .

-C 使能压缩选项 .

-4 强行使用 IPV4 地址 .

-6 强行使用 IPV6 地址 .
```