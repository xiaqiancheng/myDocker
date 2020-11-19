ARG LNMP_PHP72_VERSION
FROM php:${LNMP_PHP72_VERSION}
MAINTAINER goozp "gzp@goozp.com"

# 设置时区
ENV TZ=Asia/shanghai
RUN ln -snf /usr/share/zone/$TZ /etc/localtime && echo $TZ > /etc/timezone

# copy文件
COPY ./pkg/memcache.tgz /home/memcache.tgz

COPY ./apt/sources.list /etc/apt/sources.list

# 更新安装依赖包和php核心扩展
RUN apt-get update
RUN apt-get install -y git libfreetype6-dev libjpeg62-turbo-dev libpng-dev libmemcached-dev zlib1g zlib1g-dev
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd zip pdo_mysql opcache mysqli bcmath
RUN apt-get install -y cron
RUN apt-get install -y supervisor

RUN pecl install igbinary && echo "extension=igbinary.so" > /usr/local/etc/php/conf.d/igbinary.ini

###########################################################################
# PHP REDIS EXTENSION
###########################################################################

ARG INSTALL_PHPREDIS=false

RUN if [ ${INSTALL_PHPREDIS} = true ]; then \
    # Install Php Redis Extension
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install -o -f redis-4.3.0; \
    else \
      pecl install -o -f redis; \
    fi \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
;fi

###########################################################################
# Swoole EXTENSION
###########################################################################

ARG INSTALL_SWOOLE=false

RUN if [ ${INSTALL_SWOOLE} = true ]; then \
    # Install Php Swoole Extension
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install swoole-2.0.10; \
    else \
      if [ $(php -r "echo PHP_MINOR_VERSION;") = "0" ]; then \
        pecl install swoole-2.2.0; \
      else \
        pecl install swoole; \
      fi \
    fi && \
    docker-php-ext-enable swoole \
    && php -m | grep -q 'swoole' \
;fi

# RUN curl https://business.swoole.com/static/loader2.0.0/swoole_loader72.so > /usr/local/lib/php/extensions/no-debug-non-zts-20170718/swoole_loader72.so \
#     && { echo "extension=swoole_loader72.so"; \
#          echo "swoole_license_files=/data/www/xxx/license.zl"; } > /usr/local/etc/php/conf.d/swoole_loader.ini

ARG INSTALL_MEMCACHED=false

RUN if [ ${INSTALL_MEMCACHED} = true ]; then \
    # Install the php memcached extension
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install memcached-2.2.0; \
    else \
      pecl install memcached-3.1.3; \
    fi \
    && docker-php-ext-enable memcached \
;fi

# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

RUN apt-get install -y procps
RUN rm -r /var/lib/apt/lists/*

WORKDIR /data/www

RUN usermod -u 1000 www-data

CMD ["php-fpm"]

EXPOSE 9000
