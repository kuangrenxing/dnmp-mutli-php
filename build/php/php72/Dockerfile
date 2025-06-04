FROM php:7.2-fpm
MAINTAINER owen "chenxin448@126.com"

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 更新安装依赖包和PHP核心拓展
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

# 修改GD配置参数（旧版PHP需要指定目录路径）
RUN docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install -j$(nproc) zip
RUN docker-php-ext-install -j$(nproc) pdo
RUN docker-php-ext-install -j$(nproc) pdo_mysql
RUN docker-php-ext-install -j$(nproc) mysqli
RUN docker-php-ext-install -j$(nproc) bcmath
RUN apt-get install -y libonig-dev
# 添加 gpgme 运行时库和开发工具
RUN apt-get install -y libgpgme11 pkg-config
RUN apt-get install -y libgpgme11-dev
RUN docker-php-ext-install -j$(nproc) mbstring
RUN rm -r /var/lib/apt/lists/*
# 修改后的 gnupg 安装命令（确保依赖已加载）
RUN pecl install gnupg && echo "extension=gnupg.so" > /usr/local/etc/php/conf.d/gnupg.ini


# 安装 PECL 拓展，这里我们安装的是Redis
# 安装 Redis 扩展（指定兼容版本）
RUN pecl install redis-5.3.7 && docker-php-ext-enable redis
# 安装 mongodb
RUN #pecl install mongodb && docker-php-ext-enable mongodb
# 安装 swoole todo 未开启
RUN #pecl install swoole
#RUN pecl install swoole && docker-php-ext-enable swoole
#xdebug安装
RUN pecl install -f xdebug-2.6.1 && docker-php-ext-enable xdebug
# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

WORKDIR /var/www

# Write Permission
RUN usermod -u 1000 www-data
