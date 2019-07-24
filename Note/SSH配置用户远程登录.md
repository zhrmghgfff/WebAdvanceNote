[TOC]

# SSH配置远程登录

## 检测openssh-server安装

* 检测

```
yum list installed | grep openssh-server
```

* 安装

```
yum install openssh-server
```

## 配置SSH配置文件

* 打开sshd_config文件

```
vi /etc/ssh/sshd_config
```

* 去掉对应的注释

```
RSAAuthentication yes                    #yes，设置开启使用RSA算法的基于rhosts的安全验证；
PubkeyAuthentication yes                 #yes，设置开启公钥验证
AuthorizedKeyFiles .ssh/authorized_keys  #上传的公钥所保存的文件
StrictModes no                           #设置关闭ssh在接收登录请求之前先检查用户家目录和rhosts文件的权限和所有权
```

* 添加用户SSH登录限制

```
AllowUsers user        #允许那些用户SSH登录，多个用户空格分格
```

## 用户配置公钥

* 以用户身份登录
* 创建.ssh目录，修改访问权限

```
mkdir ~/.ssh/
chmod 700 ~/.ssh/
```

* 创建authorized_keys文件，用于存储公钥

```
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

* 复制公钥到authorized_keys文件

## 重启sshd服务
```
systemctl restart sshd.service
```

## 配置本地ssh config文件

* 添加~/.ssh/config的配置

```
#user
Host user
HostName 服务端地址
Port 22
User user 
PreferredAuthentications publickey
IdentityFile ~/.ssh/xxx
```

* 登录

```
ssh user  #user是你config配置的Host名字
```

## PS错误

```
Received disconnect from *.*.*.* port 22:2: Too many authentication failures
```

添加秘钥到ssh，即可解决

```
ssh-add xxx(私钥的文件名)
```
