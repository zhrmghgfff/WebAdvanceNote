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

> **u** 表示该文件的拥有者，**g** 表示与该文件的拥有者属于同一个群体(group)者，**o** 表示其他以外的人，**a** 表示这三者皆是
> **+** 表示增加权限、**-** 表示取消权限、**=** 表示唯一设定权限
> **r** 表示可读取，**w** 表示可写入，**x** 表示可执行，**X** 表示只有当该文件是个子目录或者该文件已经被设定过为可执行。

例：其他人增加可读权限
```
chmod o+r file
```


## 文件操作

* 删除
    
```
rm -r /home/test
rm -rf /home/test
```

* 解压

```
unzip filename.zip

tar -zxvf filename.tar.gz
tar -jxvf filename.tar.bz2
tar -Jxvf filename.tar.xz
tar -Zxvf filename.tar.Z

//从1.15版本开始tar就可以自动识别压缩的格式,故不需人为区分压缩格式就能正确解压
tar -xvf filename.tar.gz
tar -xvf filename.tar.bz2
tar -xvf filename.tar.xz
tar -xvf filename.tar.Z
```

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

## 关机

```
shutdown -h 10          #计算机将于10分钟后关闭，且会显示在登录用户的当前屏幕中

shutdown -h now         #计算机会立刻关机

shutdown -h 22:22       #计算机会在这个时刻关机

shutdown -r now         #计算机会立刻重启

shutdown -r +10         #计算机会将于10分钟后重启

reboot                  #重启

halt                    #关机
```

## 查看文件大小
* 查看目前所有文件系统的可用空间及使用情形

```
df -h
```

* 查看文件或文件夹的磁盘使用空间


```
du -h --max-depth=1 your_dest_dir
```

要想返回更深层的文件夹的大小，可以设置--max-depth为更高的数值，或者干脆取消--max-depth参数，这样它就会返回目标文件夹下所有子文件夹的大小，不管其深度（但仍不会返回文件大小，其实，想看文件大小，直接在目标目录下运行命令 ls -htla就可以啦！）

## 命令行快捷键
* 清空屏幕快捷键 `ctrl + l`
* 清空当前输入快捷键 `ctrl + u`
* 放弃当前输入快捷键 `ctrl + z`

## 防火墙

* 查看已打开的端口 
     
```
netstat -anp
```
* 查看想开的端口是否已开 
  
```
firewall-cmd --query-port=666/tcp
#若此提示 FirewallD is not running 
#表示为不可知的防火墙 需要查看状态并开启防火墙
```
  
* 查看防火墙状态

```
systemctl status firewalld
#running 状态即防火墙已经开启
#dead 状态即防火墙未开启
```
 
* 开启防火墙
   
```
systemctl start firewalld 
#没有任何提示即开启成功

#开启防火墙 
service firewalld start
#关闭防火墙 
systemctl stop firewalld

#centos7.3 上述方式可能无法开启，可以先
systemctl unmask firewalld.service
然后 
systemctl start firewalld.service
```
   
* 查看想开的端口是否已开 

```
firewall-cmd --query-port=666/tcp
#提示no表示未开
```    

* 开永久端口号 

```
firewall-cmd --add-port=666/tcp --permanent
#提示 success 表示成功
```   
* 重新载入配置 ，比如添加规则之后，需要执行此命令

```
firewall-cmd --reload
```
* 再次查看想开的端口是否已开

```
firewall-cmd --query-port=666/tcp
 #提示yes表示成功
``` 

* 若移除端口

```
firewall-cmd --permanent --remove-port=666/tcp
```

* 修改iptables
  有些版本需要安装iptables-services `yum install iptables-services` 然后修改进目录 /etc/sysconfig/iptables   修改内容
  
## 进程
### 查看进程
ps
### 杀进程

```
代号  名称      内容
1   SIGHUP	启动被终止的程序，可让该进程重新读取自己的配置文件，类似重新启动。
2   SIGINT	相当于用键盘输入 [ctrl]-c 来中断一个程序的进行。
9   SIGKILL	代表强制中断一个程序的进行，如果该程序进行到一半，那么尚未完成的部分可能会有“半产品”产生，类似 vim会有 .filename.swp 保留下来。
15  SIGTERM	以正常的方式来终止该程序。由于是正常的终止，所以后续的动作会将他完成。不过，如果该程序已经发生问题，就是无法使用正常的方法终止时，输入这个 signal 也是没有用的。
19  SIGSTOP	相当于用键盘输入 [ctrl]-z 来暂停一个程序的进行。
```

```
kill -15 pid
等于
kill -SIGTERM pid
```

## Centos 切换模式
* 获取当前模式

```
systemctl get-default
```

* 更改模式命令

```
//更改为图形界面模式
systemctl set-default graphical.target  

//更改为命令行模式
systemctl set-default multi-user.target 
```

* 重启验证

```
shutdown -r now
```