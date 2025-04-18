FROM php:7.4-fpm
MAINTAINER owen "chenxin448@126.com"


# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y git
RUN apt-get install -y vim
RUN apt-get install -y procps
RUN apt-get install -y curl
RUN apt-get install -y wget
RUN apt-get install -y lsof
RUN apt-get install -y libzip-dev zip unzip
RUN apt-get install -y libfreetype6-dev
RUN apt-get install -y libjpeg62-turbo-dev
RUN apt-get install -y libpng-dev
RUN apt-get install -y libssl-dev
RUN apt-get install -y libxml2-dev
RUN apt-get install -y libjpeg-dev
RUN apt-get install -y iputils-ping
RUN apt-get install -y libgpgme11-dev
RUN apt-get install -y libboost-all-dev
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install -j$(nproc) zip
RUN docker-php-ext-install -j$(nproc) pdo
RUN docker-php-ext-install -j$(nproc) pdo_mysql
RUN docker-php-ext-install -j$(nproc) mysqli
RUN docker-php-ext-install -j$(nproc) bcmath
RUN apt-get install -y libonig-dev
RUN docker-php-ext-install -j$(nproc) mbstring
RUN rm -r /var/lib/apt/lists/*
RUN pecl install gnupg && echo "extension=gnupg.so" > /usr/local/etc/php/conf.d/gnupg.ini


# 将预先下载好的拓展包从宿主机拷贝进去
#COPY ./pkg/redis.tgz /home/redis.tgz

#xdebug安装
#RUN pecl install xdebug && docker-php-ext-enable xdebug

# 安装 PECL 拓展，这里我们安装的是Redis
#RUN pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini
RUN pecl install redis && docker-php-ext-enable redis


#xdebug安装
RUN pecl install -f xdebug-2.9.2 && docker-php-ext-enable xdebug

# install swoole
#RUN pecl install swoole && docker-php-ext-enable swoole
#RUN mkdir -p ~/build  \
#    && cd ~/build  \
#    && rm -rf ./swoole-src  \
#    && wget -c https://github.com/swoole/swoole-src/archive/master.tar.gz -O ./swoole.tar.gz \
#    && tar zxvf ./swoole.tar.gz  \
#    && mv swoole-src* swoole-src  \
#    && cd swoole-src  \
#    && phpize  \
#    && ./configure \
#    --enable-sockets \
#    --enable-openssl --with-php-config=/usr/local/bin/php-config \
#    --enable-http2 \
##    --enable-mysqlnd && \
#    --enable-json \
#    --enable-curl \
#    && make \
#    && make install \
#    && echo "extension=swoole.so" > /usr/local/etc/php/conf.d/swoole.ini \
#    # 关闭 swoole 短名称，使用 Hyperf 这个是必须要
#    && echo "swoole.use_shortname = off" >> /usr/local/etc/php/conf.d/99-off-swoole-shortname.ini

# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

WORKDIR /var/www

# Write Permission
RUN usermod -u 1000 www-data
