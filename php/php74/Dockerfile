FROM alpine:3.15

LABEL maintainer="japool" version="1.0" license="MIT"

WORKDIR /var/www/html


# 安装 Composer
# COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# RUN chmod +x /usr/bin/composer \
#       && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

# RUN wget https://mirrors.tencent.com/composer/composer.phar \
#     && mv composer.phar  /usr/local/bin/composer \
#     && chmod +x /usr/local/bin/composer \
#     && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

RUN wget https://mirrors.aliyun.com/composer/composer.phar \
    && mv composer.phar  /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer 

# 时区更新
RUN apk add -U tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && apk del tzdata

#安装依赖扩展
ENV PHPIZE_DEPS="autoconf dpkg-dev dpkg file g++ gcc php7-dev php7-pear libc-dev make pkgconf re2c pcre-dev pcre2-dev zlib-dev libtool automake openssh libmcrypt-dev" \
    SWOOLE_VERSION=${SWOOLE_VERSION:-"4.8.11"} \
    XLSWRITER_VERSION=${XLSWRITER_VERSION:-"1.5.2"} \ 
    TZ=Asia/Shanghai

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#apt 镜像更新
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' > /etc/apk/repositories

#安装php 以及常规扩展
RUN apk update \
    && apk add --no-cache \
    # Install base packages ('ca-certificates' will install 'nghttp2-libs')
    ca-certificates \
    curl \
    wget \
    tar \
    xz \
    libressl \
    tzdata \
    pcre \
    php7 \
    php7-fpm \
    php7-apache2 \
    php7-bcmath \
    php7-bz2 \
    php7-calendar \
    php7-curl \
    php7-ctype \
    php7-dba \
    php7-dbg \
    php7-doc \
    php7-dom \
    php7-fileinfo \
    php7-exif \
    php7-gettext \
    php7-gmp \
    php7-iconv \
    php7-json \
    php7-gd \
    php7-mbstring \
    php7-mysqli  \
    php7-mysqlnd \
    php7-odbc \
    php7-opcache \
    php7-openssl \
    php7-pcntl \
    php7-pdo \
    php7-pdo_dblib \
    php7-pdo_mysql \
    php7-pdo_odbc \
    php7-pdo_pgsql \
    php7-pdo_sqlite \
    php7-pear \
    php7-pecl-amqp \
    php7-pecl-apcu \
    php7-pecl-mcrypt \
    php7-pecl-memcache \
    php7-pecl-memcached \
    php7-pecl-mongodb \
    php7-pecl-redis \
    php7-pecl-ssh2 \
    php7-phar \
    php7-posix \
    php7-session \
    php7-shmop \
    php7-simplexml \
    php7-snmp\
    php7-soap  \
    php7-sockets \
    php7-sodium \
    php7-sqlite3  \
    php7-sysvshm \
    php7-sysvmsg \
    php7-sysvsem \
    php7-tokenizer \
    php7-xml \
    php7-xmlreader \
    php7-xmlrpc \
    php7-xmlwriter \
    php7-xsl \
    php7-zip \
    && ln -sf /usr/bin/php7 /usr/bin/php \
    && apk del --purge *-dev \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/share/php7 \
    && php -v \
    && php -m 

#安装php其他扩展
RUN apk add --no-cache libstdc++ openssl bash \
    && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS libaio-dev openssl-dev curl-dev c-ares-dev 

# swoole
RUN cd /tmp \
    && wget http://pear.php.net/go-pear.phar -O go-pear.php \
    && php go-pear.php \
    # && curl -SL "https://github.com/swoole/swoole-src/archive/${SWOOLE_VERSION}.tar.gz" -o swoole.tar.gz \ 
    && pecl install --configureoptions 'enable-sockets="yes" enable-openssl="yes" enable-http2="yes" enable-mysqlnd="yes" enable-swoole-json="yes" enable-swoole-curl="yes" enable-cares="yes"' https://pecl.php.net/get/swoole-${SWOOLE_VERSION}.tgz \
    && echo "memory_limit=1G" > /etc/php7/conf.d/00_default.ini \
    && echo "opcache.enable_cli = 'On'" >> /etc/php7/conf.d/00_opcache.ini \
    && echo "extension=swoole.so" > /etc/php7/conf.d/50_swoole.ini \
    && echo "swoole.use_shortname = 'Off'" >> /etc/php7/conf.d/50_swoole.ini
# xlswriter
    && pecl install xlswriter
#RUN echo "http://nl.alpinelinux.org/alpine/edge/testing" > /etc/apk/repositories \
#    && apk add --no-cache php7-pecl-xlswriter

# 配置compose 源
RUN composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

EXPOSE 9000

CMD ["php-fpm7", "-F"]