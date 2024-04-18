FROM php:7.2-fpm
MAINTAINER owen "chenxin448@126.com"

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
        git \
        vim \
        procps \
        curl \
        wget \
        lsof \
        libzip-dev zip unzip \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libssl-dev \
        libxml2-dev \
        libjpeg-dev \
        iputils-ping \
        libgpgme11-dev \
        libmemcached-dev libmsgpack-dev libmsgpackc2 zlib1g-dev  \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-install -j$(nproc) gd zip pdo_mysql opcache mysqli bcmath soap sockets \
        && rm -r /var/lib/apt/lists/* \
        &&  pecl install gnupg && echo "extension=gnupg.so" > /usr/local/etc/php/conf.d/gnupg.ini \
        &&  pecl install memcached && echo "extension=memcached.so" > /usr/local/etc/php/conf.d/memcached.ini

#xdebug安装
RUN pecl install -f xdebug-2.6.1 && docker-php-ext-enable xdebug
# 安装 PECL 拓展，这里我们安装的是Redis
RUN pecl install redis && docker-php-ext-enable redis
# 安装 mongodb
RUN pecl install mongodb && docker-php-ext-enable mongodb
# 安装 swoole todo 未开启
RUN #pecl install swoole
#RUN pecl install swoole && docker-php-ext-enable swoole

# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

WORKDIR /var/www

# Write Permission
RUN usermod -u 1000 www-data
