# dnmp-mutli-php
DNMP（Docker + Nginx + MySQL + PHP7/5多版本 + Redis）是一款全功能的LNMP一键安装程序。

## 放在/var/www/目录下
```ini
├── build
│   ├── mongodb
│   │   └── Dockerfile
│   ├── mysql
│   │   └── Dockerfile
│   ├── nginx
│   │   └── Dockerfile
│   ├── php
│   │   ├── php56
│   │   │   ├── Dockerfile
│   │   │   └── sources.list.jessie
│   │   ├── php71
│   │   │   ├── Dockerfile
│   │   │   ├── sources.list.jessie
│   │   │   └── sources.list.stretch
│   │   └── php73
│   │       ├── Dockerfile
│   │       ├── etc
│   │       ├── sources.list.jessie
│   │       └── sources.list.stretch
│   └── redis
│       └── Dockerfile
├── conf
│   ├── mysql
│   │   ├── my.cnf
│   │   └── mysql.cnf
│   ├── nginx
│   │   ├── conf.d
│   │   │   └── default.conf
│   │   └── nginx.conf
│   ├── php56
│   │   ├── php
│   │   │   └── php.ini
│   │   └── php-fpm.d
│   │       └── www.conf
│   ├── php71
│   │   ├── php
│   │   │   └── php.ini
│   │   └── php-fpm.d
│   │       └── www.conf
│   ├── php73
│   │   ├── php
│   │   │   └── php.ini
│   │   └── php-fpm.d
│   │       └── www.conf
│   └── redis
│       └── redis.conf
├── data
│   ├── mongodb
│   ├── mysql
│   └── redis
├── docker-compose.yml
├── logs
│   ├── mysql
│   ├── nginx
│   │   ├── access.log
│   │   └── error.log
│   ├── php56
│   ├── php71
│   └── php73
├── README.md
└── site
    ├── site56
    │   └── index.php
    ├── site71
    │   └── index.php
    └── site73
        └── index.php

```
运行：注意加sudo
```bash
sudo docker-compose up
```

增加虚拟主机方法：conf/nginx/conf.d创建site71.conf文件内容：

```bash
server {
    listen       80;
    server_name  localhost;
    root   /var/www/docker/dnmp-mutli-php/site/site71;
    index  index.php index.html index.htm;
    #charset koi8-r;
    
    access_log /dev/null;
    #access_log  /var/log/nginx/nginx.localhost.access.log  main;
    error_log  /var/log/nginx/nginx.localhost.error.log  warn;
    
    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    location ~ \.php$ {
        fastcgi_pass   php71:9000;
        fastcgi_index  index.php;
        include        fastcgi_params;
        fastcgi_param  PATH_INFO $fastcgi_path_info;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}

```


/etc/hosts
添加
```bash
127.0.0.1 site56.test
127.0.0.1 site71.test
127.0.0.1 site73.test
```
在浏览器打开
```bash
http://site56.test
http://site71.test
http://site73.test
```
参考文档
```bash
https://github.com/yeszao/dnmp
https://github.com/Tinywan/dnmp
https://github.com/c0priwolf/docker-lnmp-with-mutli-php-versions
```
