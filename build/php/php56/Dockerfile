FROM php:5.6.32-fpm
MAINTAINER owen "chenxin448@126.com"

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
        git \
        vim \
        curl \
        wget \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        iputils-ping \
        libssl-dev \
        libmcrypt-dev \
        libxml2-dev \
        libgpgme11-dev \
        doxygen \
        cmake \
        libmemcached-dev \
        libevent-dev \
        zip unzip \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install zip \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli mysql sockets mcrypt mbstring soap exif\
        && docker-php-ext-install bcmath \
        && rm -r /var/lib/apt/lists/*

# 将预先下载好的拓展包从宿主机拷贝进去
#COPY ./pkg/redis.tgz /home/redis.tgz

# 安装 PECL 拓展，这里我们安装的是Redis
#RUN pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini
RUN pecl install redis-4.3.0 && docker-php-ext-enable redis

RUN pecl install memcache-3.0.8 && docker-php-ext-enable memcache
RUN pecl install memcached-2.2.0 && docker-php-ext-enable memcached

RUN pecl install xdebug-2.5.0 && docker-php-ext-enable xdebug

# install swoole
#RUN pecl install swoole && docker-php-ext-enable swoole

# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH


WORKDIR /var/www

# Write Permission
RUN usermod -u 1000 www-data
