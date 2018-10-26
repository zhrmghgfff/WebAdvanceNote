[TOC]

# SSH链接主机

## Mac系统

* 生成秘钥

```
ssh-keygen -t rsa -C "your_email@example.com"
```

输入一个名字

```
Enter file in which to save the key (/Users/malimin/.ssh/id_rsa):your_key_name
```

输入一个密码（可为空）
```
Enter passphrase (empty for no passphrase):
```

* 输出ssh的公钥

```
cat xxx.pub
```
复制到对应的地方

```
阿里云：控制台 - 云服务器ECS - 网络与安全 - 秘钥对 - 创建秘钥对
```

* 配置ssh

进入ssh目录（~/.ssh），修改config文件

```
Host ecs    // 输入ECS实例的名称
HostName *.*.*.*   // 输入ECS实例的公网IP地址
Port 22   // 输入端口号，默认为22
User root   // 输入登录账号
IdentityFile ~/.ssh/key-name // 输入ssh-key私钥文件在本机的地址
```

* 重启ssh

停止ssh
```
sudo launchctl unload -w /System/Library/LaunchDaemons/ssh.plist
```

启动ssh

```
sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist
```

* 通过ssh登录

```
ssh ecs(前面配置的名字)
```

## Windows

参看[Git](https://github.com/zhrmghgfff/WebAdvanceNote/blob/master/Note/Git.md)，通过Git Bash，操作与Mac下基本一致。

## [阿里云官方文档](https://help.aliyun.com/document_detail/51798.html?spm=a2c4g.11186623.6.674.659d4f0fZ5K0QY#linux)

