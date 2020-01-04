[TOC]

# 使用git

## 下载git

* [官方地址](https://git-scm.com/downloads)

## Windows配置SSH-Key

* 打开 Git Bash，输入如下命令

```
ssh-keygen -t rsa -C "your_email@example.com"
```

  **PS**:如果需要不使用默认命名，在回车后，输入需要的文件名

* 将SSH-Key添加到GitHub账户

```
cat xxx.pub
```
  输出秘钥，复制，添加到GitHub账户的账号设置里面

* 启动SSH客户端

```
eval $(ssh-agent -s)
```

* 添加秘钥进SSH客户端

```
ssh-add xxx(私钥的文件名)
```

* 测试连接

```
ssh -T git@github.com
```

  通过的结果：
  
```
The authenticity of host 'github.com (192.30.253.113)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.253.113' (RSA) to the list of known hosts.
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```

## Mac配置SSH-Key

步骤基本一致

## 命令

[命令行命令](https://www.jianshu.com/p/d220c88bb516)