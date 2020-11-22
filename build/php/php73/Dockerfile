FROM php:7.3-fpm
MAINTAINER owen "chenxin448@126.com"

# 编译参数，用于指定 Swoole 版本
ARG swoole_ver="v4.5.3"
# 保存到环境变量，如果没有传递就给默认值
ENV SWOOLE_VER=${swoole_ver:-"v4.5.3"}

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
        autoconf \
#        openssl-dev \
#        build-base \
#        zlib-dev \
#        re2c \
#        oniguruma-dev \
        libzip-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libjpeg-dev \
        libpng-dev \
        iputils-ping \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install zip \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli \
        && docker-php-ext-install bcmath \
        && docker-php-ext-install pcntl \
        && docker-php-ext-install sockets \
        && rm -r /var/lib/apt/lists/*

# 将预先下载好的拓展包从宿主机拷贝进去
#COPY ./pkg/redis.tgz /home/redis.tgz

#xdebug安装 目前手动关闭 开启编辑vim /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN pecl install xdebug && docker-php-ext-enable xdebug

# 安装 PECL 拓展，这里我们安装的是Redis
#RUN pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini
RUN pecl install redis && docker-php-ext-enable redis

RUN pecl install mongodb && docker-php-ext-enable mongodb

# install swoole
RUN pecl install swoole && docker-php-ext-enable swoole

# install sdebug
# 运行克隆前，先把目录切换到 /tmp ，避免之前的命令导致目录错误
RUN cd /tmp \
    # from mirrors
    && git clone https://gitee.com/vyi/sdebug sdebug \
    # 进入克隆的目录
    && cd sdebug \
    # 切换到 sdebug_2_7 分支，这里一定到切换分支，因为 master 分支是 Xdebug 的源码
    && git checkout sdebug_2_7 \
    && phpize \
    && ./configure --enable-xdebug \
    && make \
    && make install \
    # 这里 安装完成后执行的值 xdebug
    && docker-php-ext-enable xdebug \
    && php -m \
    # 这里检查也是哟,注意是 sdebug
    && php --ri sdebug


# config php
RUN cd /usr/local/etc/php/conf.d \
    # swoole config
    # 关闭 swoole 短名称，使用 Hyperf 这个是必须要
    && echo "swoole.use_shortname = off" >> 99-off-swoole-shortname.ini \
    # config xdebug
    && { \
        # 添加一个 Xdebug 节点
        echo "[Xdebug]"; \
        # 启用远程连接
        echo "xdebug.remote_enable = 1"; \
        # 这个是多人调试，但是现在有些困难，就暂时不启动
        echo ";xdebug.remote_connect_back = On"; \
        # 自动启动远程调试
        echo "xdebug.remote_autostart  = true"; \
        # 这里 host 可以填前面取到的 IP ，也可以填写 host.docker.internal 。
        echo "xdebug.remote_host = host.docker.internal"; \
        # 这里端口固定填写 19000 ，当然可以填写其他的，需要保证没有被占用
        echo "xdebug.remote_port = 9000"; \
        # 这里固定即可
        echo "xdebug.idekey=PHPSTORM"; \
        # 把执行结果保存到 99-xdebug-enable.ini 里面去
    } | tee 99-xdebug-enable.ini


# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH


WORKDIR /var/www

# Write Permission
RUN usermod -u 1000 www-data
