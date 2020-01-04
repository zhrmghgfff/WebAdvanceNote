# Nginx配置详解

## 必备工具
* GCC编译器

```
//gcc安装
yum install -y gcc
//g++安装
yum install -y gcc-c++
```
* PCRE库（Perl兼容正则表达式）、pcre-devel（PCRE二次开发）

```
yum install -y pcre pcre-devel
```
* zlib库（gzip压缩）

```
yum install -y zlib zlib-devel
```
* OpenSSL开发库

```
yum install -y openssl openssl-devel
```
## 源码
[nginx官网](http://nginx.org/)
## 官方文档
[中文文档](http://www.nginx.cn/doc/index.html)
[nginx配置说明](http://seanlook.com/2015/05/17/nginx-install-and-config/)
[nginx配置location总结及rewrite规则写法](http://seanlook.com/2015/05/17/nginx-location-rewrite/)

## 常用命令
* 快速关闭Nginx，可能不保存相关信息，并迅速终止web服务

```
nginx -s stop
```

* 平稳关闭Nginx，保存相关信息，有安排的结束web服务

```
nginx -s quit
```

* 因改变了Nginx相关配置，需要重新加载配置而重载

```
nginx -s reload
```

* 重新打开日志文件

```
nginx -s reopen
```

* 为 Nginx 指定一个配置文件，来代替缺省的

```
nginx -c filename
```

* 不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件

```
nginx -t
```

*  显示 nginx 的版本

```
nginx -v
```

* 显示 nginx 的版本，编译器版本和配置参数

```
nginx -V
```

* 格式换显示 nginx 配置参数

```
2>&1 nginx -V | xargs -n1
2>&1 nginx -V | xargs -n1 | grep lua
```