[TOC]

# Docker配置Gitlab

## 下载镜像
[DockerHub](https://hub.docker.com/)里面有各种镜像

gitlab-ce

```
docker pull gitlab/gitlab-ce
```

下载完毕，**docker images** 命令查看镜像

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
gitlab/gitlab-ce    latest              cad746e90246        8 hours ago         1.84GB
```

记住镜像id，之后要用到

## run 容器

```
docker run --detach --hostname gitlab.xxx.com --publish 444:443 --publish 81:80 --publish 23:22 --name gitlab --restart always --volume /web/gitlab/config:/etc/gitlab --volume /web/gitlab/logs:/var/log/gitlab --volume /web/gitlab/data:/var/opt/gitlab cad746e90246
```
* --hostname ：指定容器中绑定的域名，会在创建镜像仓库的时候使用到，这里绑定gitlab.xxx.com
* --publish：端口映射；容器内的443,80,22端口分别映射到宿主机的444,81,23端口
* --volume ：挂载数据卷，映射到容器中去的容器外部存储空间
* cad746e90246 ：镜像的ID

数据存储地方

| 当地的位置 | 容器的位置 | 作用 |
| --- | --- | --- |
| /web/gitlab/config | /etc/gitlab | 用于存储GitLab配置文件 |
| /web/gitlab/logs | /var/log/gitlab | 用于存储日志 |
| /web/gitlab/data | /var/opt/gitlab | 用于存储应用数据 |

通过 **docker ps** 命令看到gitlab容器证明你已经运行成功了

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                             PORTS                                                          NAMES
80a875dbd302        cad746e90246        "/assets/wrapper"   14 seconds ago      Up 13 seconds (health: starting)   0.0.0.0:23->22/tcp, 0.0.0.0:81->80/tcp, 0.0.0.0:444->443/tcp   gitlab
```

## 配置Gitlab
配置都在唯一的配置文件 **/etc/gitlab/gitlab.rb**

进入容器内部通过shell会话进行相关操作

```
docker exec -it gitlab /bin/bash
```

### SMTP Setting

GitLab的使用过程中涉及到大量的邮件，而邮件服务你可以选择使用Postfix,sendmai,SMTP服务其中一种，SMTP相对简单

#### 编辑**/etc/gitlab/gitlab.rb**
加到文件最后面就好了

```
gitlab_rails['smtp_enable'] = true

gitlab_rails['smtp_address'] = "smtp.163.com"

gitlab_rails['smtp_port'] = 25

gitlab_rails['smtp_user_name'] = "XXX@163.com"

gitlab_rails['smtp_password'] = "password"

gitlab_rails['smtp_domain'] = "163.com"

gitlab_rails['smtp_authentication'] = :login

gitlab_rails['smtp_enable_starttls_auto'] = true

gitlab_rails['gitlab_email_from'] = "XXX@163.com"

user["git_user_email"] = "XXX@163.com"
```
* gitlab_rails['smtp_address'] ：SMTP服务地址，不同的服务商不同
* gitlab_rails['smtp_port'] ：服务端口
* gitlab_rails['smtp_user_name'] ：用户名，自己注册的
* gitlab_rails['smtp_password'] ：客户端授权秘钥（获取方式，下图讲解）
* gitlab_rails['gitlab_email_from'] ：发出邮件的用户，注意跟用户名保持一致
* user["git_user_email"] ：发出用户，注意跟用户名保持一致

#### 获取邮箱客户端秘钥
登录163邮箱，设置->客户端授权密码->开启，设置授权密码，勾选SMTP

#### 重新加载gitlab的配置

```
gitlab-ctl reconfigure
```
#### 测试邮件

```
gitlab-rails console
Notify.test_email('xxxx@qq.com', '测试邮件', 'gitlab邮件').deliver_now
```
## 登录

通过81端口登录

```
ip:81
```

## 拉取代码
当主机配置有gitlab时，使用一般的ip或者域名请求，会报错
原因：
1. 本地配有秘钥用户ssh登录主机，config需要配置的域名（或IP）
2. gitlab也需要配置ssh，config需要配置的域名（或IP）
3. 两者的域名（或IP）是一样的

解决方案：修改拉取代码的clone语句

**原语句**
```
git clone ssh://git@*.*.*.*:端口/myproject.git
```

`~/.ssh/config`
```
Host myssh    // 输入ECS实例的名称
HostName *.*.*.*   // 输入ECS实例的公网IP地址
Port 22   // 输入端口号，默认为22
User git   // 输入登录账号
IdentityFile ~/.ssh/key-name // 输入ssh-key私钥文件在本机的地址
```

**修改后的语句**
```
git clone myssh:/myproject.git
```

### Bad owner or permissions
```
Bad owner or permissions on ~/.ssh/config
```
结局方案

```
chmod 600 ~/.ssh/config
```
**PS** `chmod 777`问什么不行，权限不是更高吗
某些更高级别的权限会导致系统变得不安全。将导致某些程序退出。


## 参考
[使用docker搭建gitlab环境](https://segmentfault.com/a/1190000014305359)
