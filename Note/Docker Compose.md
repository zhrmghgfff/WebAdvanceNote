[TOC]

# Docker Compose配置
## 升级python
安装环境
```
yum -y install zlib*
```

下载python，安装
```
wget https://www.python.org/ftp/python/3.8.1/Python-3.8.1rc1.tar.xz
tar xJf Python-3.8.1rc1.tar.xz
cd Python-3.8.1rc1
./configure
make
make install
```

检测
```
which python3
python3 -V
```

创建软链接
```
cd /usr/bin
mv python python.backup
ln -s /usr/local/bin/python3 /usr/bin/python
```

修改yum，yum是python2写的，python3兼容问题
修改yum配置文件，将python版本指向以前的旧版本
```
# vi /usr/bin/yum
#!/usr/bin/python2.7
```

修改urlgrabber-ext-down文件，更改python版本
```
vi /usr/libexec/urlgrabber-ext-down
#!/usr/bin/python2.7
```

## 安装 docker-compose

```
1、首先检查linux有没有安装python-pip包，直接执行 
    yum install python-pip -y
2、没有python-pip包就执行命令 
    yum -y install epel-release
3、执行成功之后，再次执行
    yum install python-pip
4、对安装好的pip进行升级 
    pip install --upgrade pip
    至此，pip工具就安装好了，下面使用pip 安装docker-compose
5、pip install docker-compose
```

报错

```
ERROR: jsonschema 3.2.0 has requirement six>=1.11.0, but you'll have six 1.9.0 which is incompatible.
ERROR: ipapython 4.6.5 has requirement dnspython>=1.15, but you'll have dnspython 1.12.0 which is incompatible.
ERROR: ipapython 4.6.5 has requirement python-ldap>=3.0.0b1, but you'll have python-ldap 2.4.15 which is incompatible.
```

依赖的版本低，根据提示升级依赖
```
pip install python-ldap --user -U
pip install six --user -U
pip install dnspython --user -U
```

python-ldap安装错误，报Python.h找不到
```
yum install python-devel   # for python2.x installs
yum install python3-devel   # for python3.x installs
```
lber.h找不到
```
yum -y install openldap-devel
```

Cannot uninstall 'subprocess32'

```
# 搜索subprocess32-3.2.6-py2.7.egg-info文件
sudo find / -name *subpro*.egg-info
# 删除
rm -rf /usr/lib64/python2.7/site-packages/subprocess32-3.2.6-py2.7.egg-info
```

Cannot uninstall 'requests'
```
# 搜索requests-2.6.0-py2.7.egg-info文件
sudo find / -name *requests*.egg-info
# 删除
rm -rf /usr/lib/python2.7/site-packages/requests-2.6.0-py2.7.egg-info
```

验证
```
docker-compose --version
```

## 编写docker-compose.yml文件

```
version: '3'
services:
    web-mysql:
        image: mysql:5.7.28
        container_name: web-mysql
        ports:
            - '3306:3306'
        volumes:
            - /web/www/mysql/:/var/lib/mysql
        environment:
            - MYSQL_ROOT_PASSWORD=2012

    web-redis:
        image: redis:5.0.7
        container_name: web-redis
        ports:
            - '6379:6379'
        volumes:
            - /web/www/redis/:/data
        
    web-php:
        image: php:7.4.0-fpm
        container_name: web-php
        privileged: true
        depends_on:
            - web-mysql
            - web-redis
        links:
            - web-mysql:web-mysql
            - web-redis:web-redis
        ports:
            - '9000:9000'
        volumes:
            - /web/www/html:/usr/share/nginx/html
        
    web-nginx:
        image: nginx:1.17.6
        container_name: web-nginx
        privileged: true
        depends_on:
            - web-php
        links: 
            - web-php:web-php
        ports:
            - '80:80'
            - '443:443'
        volumes:
            - /web/www/html:/usr/share/nginx/html
            - /web/www/nginx/conf.d:/etc/nginx/conf.d
            - /web/www/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
            - /web/www/nginx/servers:/etc/nginx/servers
            - /web/www/nginx/log:/var/log/nginx
            - /web/www/nginx/letsencrypt:/etc/nginx/letsencrypt:rw
```

##执行

```
docker-compose up -d
```

## 重启后
### mysql出错

```
Operating system error number 22 in a file operation.
Error number 22 means 'Invalid argument'
File ./ib_logfile101: 'aio write' returned OS error 122. Cannot continue operation
Cannot continue operation.
```
原因：因为使用虚拟机，外挂的目录在Mac OS系统上，导致的
解决方案，修改mysql配置
外挂配置
```
-v /web/www/mysql/conf.d:/etc/mysql/conf.d
```

conf.d文件夹下，新建一个配置文件local.cnf,内容如下
```
[mysqld]
innodb_use_native_aio=0
```

### Nginx访问不了

* 检测端口
  80/tcp
  
```
//防火墙
firewall-cmd --state

//查询端口
firewall-cmd --query-port=80/tcp
#提示no表示未开

//添加端口
firewall-cmd --add-port=80/tcp --permanent
#提示 success 表示成功

//重启防火墙
firewall-cmd --reload
```

* 手动重启防火墙

当FirewallD启动（或重新启动）时，会从iptables中删除DOCKER链，造成Docker不能正常工作
在CentOS 7中，如果设置使用systemd开机自启动Docker服务是不会有问题的，因为Docker在systemd配置文件中明确注明了“After= firewalld.service”，以保证Docker daemon 在FirewallD启动后再启动。
    
```
//重启docker守护进程
systemctl restart docker.service
```

* 没有启用IP_FORWARD

IP_FORWARD，IP转发功能，停用会导致外部无法访问容器的端口

docker容器启动也会有警告
```
WARNING: IPv4 forwarding is disabled. Networking will not work.
```

```
//查询
sysctl net.ipv4.ip_forward
```

结果
```
net.ipv4.ip_forward = 0
```

Docker daemon服务在启动的时候会自动设置iptables设置
```
//重启docker守护进程
systemctl restart docker.service
```