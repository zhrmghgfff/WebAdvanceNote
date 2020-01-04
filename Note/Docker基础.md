# Docker基础
## 基础镜像

[官方地址](https://hub.docker.com/)

## 基本使用
[参考手册](https://www.runoob.com/docker/docker-image-usage.html) [官方文档](https://docs.docker.com/)
### 本地镜像与容器的位置

```
/var/lib/docker  默认位置
```
### 查看镜像

```
docker images
```
### 执行命令

```
//nginx 执行reload
docker container exec <container> nginx -s reload
```
### 查看容器配置

```
docker inspect <container>
```

### 进入容器
```
docker exec -it gitlab /bin/bash
```

### 拷贝文件
拷贝到容器
```
docker cp /www/runoob 96f7f14e99ab:/www/
```
拷贝出容器
```
docker cp  96f7f14e99ab:/www /tmp/
```