# Laravel配置
## PHP安装
由于linux的yum源不存在php7.x，所以我们要更改yum源：

```
yum install epel-release

yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

```
yum install yum-utils
```

```
yum-config-manager --enable remi-php74
yum update
```

安装
```
yum install php74
```

## 安装composer

```
php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"
php composer-setup.php
```

移动 composer.phar，这样 composer 就可以进行全局调用：
```
mv composer.phar /usr/local/bin/composer
```

切换为国内镜像
```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

更新 composer：
```
composer selfupdate
```

## 创建laravel项目

```
composer create-project laravel/laravel cms  ^6.5
```

错误：laravel/framework v6.x.0 requires ext-mbstring
缺少对应的php扩展，安装即可
```
yum install php74-php-fpm php74-php-bcmath php74-php-ctype php74-php-json php74-php-mbstring php74-php-openssl php74-php-pdo php74-php-tokenizer php74-php-xml
```

## 配置web服务
[参考](https://www.jianshu.com/p/1de1ab48e8cb)
安装完成后，进入目录，给予storage读写权限
```
chmod -R 777 storage
```

配置nginx
```
#将root定位到工程下的public
root    /usr/share/nginx/html/cms/public;
```

全部配置
```
server {
    listen       80 default_server;
    server_name  localhost;
    
    root   /usr/share/nginx/html/cms/public;

    location / {
        index  index.html index.htm index.php;
        try_files $uri $uri/ /index.php?$query_string;
    }

    error_page  404              /404.html;
    error_page   500 502 503 504  /50x.html;

    location ~ \.php$ {
        fastcgi_pass   web-php:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```
## [开始学习](https://www.jianshu.com/p/1de1ab48e8cb)