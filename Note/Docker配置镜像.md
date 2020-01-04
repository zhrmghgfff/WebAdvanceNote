[TOC]
# 镜像地址
[DockerHub](https://hub.docker.com/)里面有各种镜像
[Docker教程](https://www.runoob.com/docker/docker-tutorial.html)
## run 命令解析

```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

* -a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；

* -d: 后台运行容器，并返回容器ID；

* -i: 以交互模式运行容器，通常与 -t 同时使用；

* -P: 随机端口映射，容器内部端口随机映射到主机的高端口

* -p: 指定端口映射，格式为：主机(宿主)端口:容器端口

* -t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；

* --name="nginx-lb": 为容器指定一个名称；

* --dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；

* --dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；

* -h "mars": 指定容器的hostname；

* -e username="ritchie": 设置环境变量；

* --env-file=[]: 从指定文件读入环境变量；

* --cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；

* -m :设置容器使用内存最大值；

* --net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；

* --link=[]: 添加链接到另一个容器；

* --expose=[]: 开放一个端口或一组端口；

* --volume , -v: 绑定一个卷



# Docker配置MySQL
## 下载镜像
```
docker pull mysql
```
## run 容器

```
docker run -itd --name web-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=mima mysql:5.7.28
```

* -itd = -i -t -d

# Docker配置Redis
## 下载镜像
```
docker pull redis
```
## run 容器

```
docker run -itd --name web-redis -p 6379:6379 redis:5.0.7
```

# Docker配置PHP
## 下载镜像
```
docker pull php
```
## run 容器

```
docker run -d -p 9000:9000 --name web-php -v /web/www/html:/var/www/html -v /web/php:/usr/local/etc/php --link web-mysql:web-mysql --link web-redis:web-redis --privileged=true php:7.4.0-fpm
```

# Docker配置Nginx

## 下载镜像

```
docker pull nginx
```

## run 容器

```
docker run \
--name web-nginx  -d \
-p 80:80 -p 443:443 \
-v /web/www:/usr/share/nginx/html \
-v /web/nginx/conf.d:/etc/nginx/conf.d \
-v /web/nginx/nginx.conf:/etc/nginx/nginx.conf:ro \
-v /web/nginx/servers:/etc/nginx/servers \
-v /web/nginx/log:/var/log/nginx \
-v /web/nginx/letsencrypt:/etc/nginx/letsencrypt:rw \
--link web-php:web-php --privileged=true \
nginx:1.17.6
```
* -v /web/nginx/conf/nginx.conf:/etc/nginx/nginx.conf：将主机中当前目录下的nginx.conf挂载到容器的/etc/nginx/nginx.conf
* -v /web/nginx/www:/opt/nginx/www：将主机中当前目录下的www挂载到容器的/opt/nginx/www，参考nginx.conf的root配置
* -v /web/nginx/log:/opt/nginx/log：将主机中当前目录下的log挂载到容器的/opt/nginx/log，参考nginx.conf的log配置

