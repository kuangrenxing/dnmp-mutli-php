# docker-compose-mutli-php
多版本php环境 php5.6 php7.1 php7.3

## 放在/var/www/目录下
```ini
├── docker-compose-mutli-php
│   ├── data
│   │   ├── mysql
│   │   └── redis
│   │       └── dump.rdb
│   ├── docker-compose.yml
│   ├── logs
│   │   ├── mysql
│   │   ├── nginx
│   │   │   ├── access.log
│   │   │   └── error.log
│   │   ├── php56-fpm
│   │   ├── php71-fpm
│   │   └── php73-fpm
│   ├── mysql
│   │   ├── conf.d
│   │   └── Dockerfile
│   ├── nginx
│   │   ├── conf.d
│   │   │   ├── default.conf
│   │   │   ├── site56.conf
│   │   │   ├── site71.conf
│   │   │   └── site73.conf
│   │   ├── Dockerfile
│   │   └── nginx.conf
│   ├── php
│   │   ├── php56-fpm
│   │   │   ├── Dockerfile
│   │   │   ├── etc
│   │   │   │   ├── php
│   │   │   │   │   ├── conf.d
│   │   │   │   │   └── php.ini
│   │   │   │   └── php-fpm.d
│   │   │   │       └── www.conf
│   │   │   └── sources.list.jessie
│   │   ├── php71-fpm
│   │   │   ├── Dockerfile
│   │   │   ├── etc
│   │   │   │   ├── php
│   │   │   │   │   ├── conf.d
│   │   │   │   │   └── php.ini
│   │   │   │   └── php-fpm.d
│   │   │   │       └── www.conf
│   │   │   ├── sources.list.jessie
│   │   │   └── sources.list.stretch
│   │   └── php73-fpm
│   │       ├── Dockerfile
│   │       ├── etc
│   │       │   ├── php
│   │       │   │   ├── conf.d
│   │       │   │   └── php.ini
│   │       │   └── php-fpm.d
│   │       │       └── www.conf
│   │       ├── sources.list.jessie
│   │       └── sources.list.stretch
│   ├── redis
│   │   └── Dockerfile
│   └── site
│       ├── site1
│       ├── site2
│       │   └── index.php
│       └── site3
│           └── index.php
├── README.md
├── site56
│   └── index.php
├── site71
│   └── index.php
└── site73
    └── index.php
```
运行：注意加sudo
```bash
sudo docker-compose up
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
