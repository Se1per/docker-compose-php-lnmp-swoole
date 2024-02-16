FROM alpine:3.19

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

# 时区更新 # http://mirrors.aliyun.com/alpine/v3.19/community/
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g'  /etc/apk/repositories
RUN apk add -U tzdata \
&& cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
&& apk del tzdata

#安装依赖扩展
ENV PHPIZE_DEPS="autoconf dpkg-dev dpkg file g++ gcc php82-dev php82-pear libc-dev make pkgconf re2c pcre-dev pcre2-dev zlib-dev libtool automake openssh libmcrypt-dev" \
SWOOLE_VERSION=${SWOOLE_VERSION:-"5.1.0"}

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
    php82 \
    php82-fpm \
    php82-apache2 \
    php82-bcmath \
    php82-bz2 \
    php82-calendar \
    php82-curl \
    php82-ctype \
    php82-dba \
    php82-dbg \
    php82-doc \
    php82-dom \
    php82-fileinfo \
    php82-exif \
    php82-gettext \
    php82-gmp \
    php82-iconv \
    php82-gd \
    php82-json \
    php82-mbstring \
    php82-mysqli  \
    php82-mysqlnd \
    php82-odbc \
    php82-opcache \
    php82-openssl \
    php82-pcntl \
    php82-pdo \
    php82-pdo_dblib \
    php82-pdo_mysql \
    php82-pdo_odbc \
    php82-pdo_pgsql \
    php82-pdo_sqlite \
    php82-pear \
    php82-pecl-amqp \
    php82-pecl-mcrypt \
    php82-pecl-memcache \
    php82-pecl-memcached \
    php82-pecl-mongodb \
    php82-pecl-redis \
    php82-pecl-ssh2 \
    php82-pecl-xlswriter \
    php82-phar \
    php82-session \
    php82-shmop \
    php82-posix \
    php82-soap  \
    php82-sockets \
    php82-sodium \
    php82-simplexml \
    php82-tokenizer \
    php82-sqlite3  \
    php82-sysvshm \
    php82-sysvmsg \
    php82-sysvsem \
    php82-zip \
    php82-xml \
    php82-xmlreader \
    php82-xmlwriter \
    php82-xsl \
    && ln -sf /usr/bin/php82 /usr/bin/php \
    && apk del --purge *-dev \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/share/php82 \
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
    && echo "memory_limit=1G" >> /etc/php82/conf.d/00_default.ini \
    && echo "opcache.enable_cli = 'On'" >> /etc/php82/conf.d/00_opcache.ini \
    && echo "extension=swoole.so" >> /etc/php82/conf.d/50_swoole.ini \
    && echo "swoole.use_shortname = 'Off'" >> /etc/php82/conf.d/50_swoole.ini \

# 配置compose 源
RUN composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

EXPOSE 9000

CMD ["php-fpm82", "-F"]