FROM php:7.4-fpm
MAINTAINER goozp "313569855@qq.com"
#设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN sed -i s@/deb.debian.org/@/mirrors.aliyun.com/@g /etc/apt/sources.list
#更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
    procps \
    cron \
    vim \
    git \
    libzip-dev \
    zip \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
&& docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
&& docker-php-ext-install -j$(nproc) gettext\
&& docker-php-ext-install -j$(nproc) sockets\
&& docker-php-ext-install -j$(nproc) gd \
&& docker-php-ext-install zip \
&& docker-php-ext-install pdo_mysql \
&& docker-php-ext-install opcache \
&& docker-php-ext-install mysqli \
&& rm -r /var/lib/apt/lists/*

RUN pecl install redis \
    && pecl install swoole \
    && docker-php-ext-enable redis swoole
#安装 Composer
COPY ./composer /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer
RUN composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
WORKDIR /data
#Write Permission
RUN usermod -u 1000 www-data