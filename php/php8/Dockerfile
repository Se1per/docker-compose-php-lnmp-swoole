FROM alpine:3.16

LABEL maintainer="japool" version="1.0" license="MIT"

WORKDIR /var/www/html

#composer
# COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# RUN chmod +x /usr/bin/composer \
#     && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
# RUN wget https://mirrors.tencent.com/composer/composer.phar \
#     && mv composer.phar  /usr/local/bin/composer \
#     && chmod +x /usr/local/bin/composer \
#     && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

RUN wget https://mirrors.aliyun.com/composer/composer.phar \
    && mv composer.phar  /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer

# 时区更新 # http://mirrors.aliyun.com/alpine/v3.16/community/
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g'  /etc/apk/repositories
RUN apk add -U tzdata \
&& cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
&& apk del tzdata

#安装依赖扩展
ENV PHPIZE_DEPS="autoconf dpkg-dev dpkg file g++ gcc php8-dev php8-pear libc-dev make pkgconf re2c pcre-dev pcre2-dev zlib-dev libtool automake openssh libmcrypt-dev" \
    SWOOLE_VERSION=${SWOOLE_VERSION:-"4.8.11"}

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
    php8 \
    php8-fpm \
    php8-apache2 \
    php8-bcmath \
    php8-bz2 \
    php8-calendar \
    php8-curl \
    php8-ctype \
    php8-dba \
    php8-dbg \
    php8-doc \
    php8-dom \
    php8-fileinfo \
    php8-exif \
    php8-gettext \
    php8-gmp \
    php8-iconv \
    php8-gd \
    php8-json \
    php8-mbstring \
    php8-mysqli  \
    php8-mysqlnd \
    php8-odbc \
    php8-opcache \
    php8-openssl \
    php8-pcntl \
    php8-pdo \
    php8-pdo_dblib \
    php8-pdo_mysql \
    php8-pdo_odbc \
    php8-pdo_pgsql \
    php8-pdo_sqlite \
    php8-pear \
    php8-pecl-amqp \
    php8-pecl-mcrypt \
    php8-pecl-memcache \
    php8-pecl-memcached \
    php8-pecl-mongodb \
    php8-pecl-redis \
    php8-pecl-ssh2 \
    php8-phar \
    php8-session \
    php8-shmop \
    php8-posix \
    php8-soap  \
    php8-sockets \
    php8-sodium \
    php8-simplexml \
    php8-tokenizer \
    php8-sqlite3  \
    php8-sysvshm \
    php8-sysvmsg \
    php8-sysvsem \
    php8-zip \
    php8-xml \
    php8-xmlreader \
    php8-xmlwriter \
    php8-xsl \
    && ln -sf /usr/bin/php8 /usr/bin/php \
    && apk del --purge *-dev \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/share/php8 \
    && php -v \
    && php -m

#安装php其他扩展
RUN apk add --no-cache libstdc++ openssl bash \
&& apk add --no-cache --virtual .build-deps $PHPIZE_DEPS build-base libaio-dev openssl-dev libxml2-dev curl curl-dev shadow c-ares-dev libjpeg-turbo-dev libpng-dev libxpm-dev freetype-dev gd-dev gettext-dev

# swoole
RUN cd /tmp \
    && wget http://pear.php.net/go-pear.phar -O go-pear.php \
    && php go-pear.php \
    # && curl -SL "https://github.com/swoole/swoole-src/archive/${SWOOLE_VERSION}.tar.gz" -o swoole.tar.gz \
    && pecl install --configureoptions 'enable-sockets="yes" enable-openssl="yes" enable-http2="yes" enable-mysqlnd="yes" enable-swoole-json="yes" enable-swoole-curl="yes" enable-cares="yes"' https://pecl.php.net/get/swoole-${SWOOLE_VERSION}.tgz \
    && echo "memory_limit=1G" > /etc/php8/conf.d/00_default.ini \
    && echo "opcache.enable_cli = 'On'" >> /etc/php8/conf.d/00_opcache.ini \
    && echo "extension=swoole.so" > /etc/php8/conf.d/50_swoole.ini \
    && echo "swoole.use_shortname = 'Off'" >> /etc/php8/conf.d/50_swoole.ini \
    # xlswriter
    && pecl install xlswriter
#RUN echo "http://nl.alpinelinux.org/alpine/edge/testing" > /etc/apk/repositories \
#&& apk add --no-cache php8-pecl-xlswriter

# 配置compose 源
RUN composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

EXPOSE 9000

CMD ["php-fpm8", "-F"]