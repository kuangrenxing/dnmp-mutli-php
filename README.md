# docker-compose-mutli-php
多版本php环境 php5.6 php7.1 php7.3

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
    listen   80;
    index index.html index.htm;
    server_name site71.test;

    root /var/www/docker/dnmp-mutli-php/site/site71;
    index index.php index.html index.htm;
    location / {
        try_files $uri $uri/ /index.html;
    }

    location ~ \.php {
        include fastcgi_params;
        fastcgi_pass   php71:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /var/www/docker/dnmp-mutli-php/site/site71/$fastcgi_script_name;
    }
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
