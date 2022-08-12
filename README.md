# docker-compose-lnmp-swoole

This repository contains a little `docker-compose` configuration to start a `LEMP (Linux, Nginx, MariaDB, PHP)` stack.

Example PHP-FPM & Nginx setup for Docker, build on [Alpine Linux](http://www.alpinelinux.org/).
The image is only +/- 35MB large.

* Built on the lightweight and secure Alpine Linux distribution
* Very small Docker image size (+/-35MB)
* Uses latest PHP version for better performance, lower CPU usage & memory footprint
* Optimized for 100 concurrent requests (optimized for container with limit 0.25 vcpu, 512 mb)
* Optimized for building micro RESTful API
* The servers Nginx, PHP-FPM and supervisord run under a non-privileged user (nobody) to make it more secure
* The logs of all the services are redirected to the output of the Docker container (visible with `docker logs -f <container name>`)
* Follows the KISS principle (Keep It Simple, Stupid) to make it easy to understand and adjust the image to your needs

## Details

The following versions are used.

* PHP 7.4 (FPM) - With MySQLi driver optionally (Uncomment line from php.Dockerfile)
* Nginx 1.22.0
* MariaDB 10.8.3
* mysql   5.7

## Configuration

You can also set the following environment variables, for example in the included `.env` file:


## Running
Start the server using the following command inside the directory you just cloned: `docker-compose up`.

```sh
docker-compose up -d

docker-compose up

```
## Entering the containers

You can use the following command to enter a container:

Where `{CONTAINER_NAME}` is one of:

`docker exec -ti {CONTAINER_NAME} /bin/bash`

* `{APP_NAME}-php`
* `{APP_NAME}-nginx`
* `{APP_NAME}-mariadb`


#### PHP extensions included:
```sh
$ php -m
[PHP Modules]
bcmath
bz2
calendar
Core
ctype
curl
date
dba
dom
exif
filter
gd
gettext
gmp
hash
iconv
igbinary
json
libxml
mbstring
mcrypt
mysqli
mysqlnd
odbc
openssl
pcntl
pcre
PDO
pdo_mysql
pdo_sqlite
Phar
phpdbg_webhelper
posix
readline
redis
Reflection
session
shmop
soap
sockets
sodium
SPL
sqlite3
standard
swoole
sysvmsg
sysvsem
sysvshm
xlswriter
xml
xmlreader
xmlwriter
xsl
Zend OPcache
zip
zlib

[Zend Modules]
Zend OPcache
```

##### Other php7 packages available in repository

```sh
$ apk --update search php7
php7-intl-7.1.3-r0
php7-openssl-7.1.3-r0
php7-dba-7.1.3-r0
php7-sqlite3-7.1.3-r0
php7-dev-7.1.3-r0
php7-pear-7.1.3-r0
php7-shmop-7.1.3-r0
php7-phpdbg-7.1.3-r0
php7-xmlwriter-7.1.3-r0
php7-pecl-7.1.3-r0
php7-posix-7.1.3-r0
php7-litespeed-7.1.3-r0
php7-gmp-7.1.3-r0
php7-pdo_mysql-7.1.3-r0
php7-bz2-7.1.3-r0
php7-pcntl-7.1.3-r0
php7-common-7.0.17-r4
php7-pdo-7.1.3-r0
php7-oauth-2.0.2-r0
php7-xsl-7.1.3-r0
php7-ctype-7.1.3-r0
php7-mbstring-7.1.3-r0
php7-fpm-7.1.3-r0
php7-mysqli-7.1.3-r0
php7-phar-utils-7.1.3-r0
php7-gmagick-2.0.4_rc1-r3
php7-imagick-3.4.3-r1
php7-mysqlnd-7.1.3-r0
php7-enchant-7.1.3-r0
php7-solr-2.4.0-r0
php7-uuid-1.0.4-r0
php7-curl-7.1.3-r0
php7-pspell-7.1.3-r0
php7-xmlrpc-7.1.3-r0
php7-imap-7.1.3-r0
php7-ast-0.1.4-r0
php7-libs-7.1.3-r0
php7-redis-3.1.2-r0
php7-phar-7.1.3-r0
php7-snmp-7.1.3-r0
php7-doc-7.1.3-r0
php7-fileinfo-7.1.3-r0
php7-opcache-7.1.3-r0
php7-sockets-7.1.3-r0
php7-lzf-1.6.5-r1
php7-xmlreader-7.1.3-r0
php7-dom-7.1.3-r0
php7-timezonedb-2017.2-r0
php7-apache2-7.1.3-r0
php7-pear-mail_mime-1.10.0-r0
php7-rdkafka-2.0.0-r0
php7-stats-2.0.3-r0
php7-embed-7.0.17-r4
php7-pdo_sqlite-7.1.3-r0
php7-pear-auth_sasl2-0.2.0-r0
php7-exif-7.1.3-r0
php7-msgpack-2.0.2-r0
php7-wddx-7.1.3-r0
php7-recode-7.1.3-r0
php7-ldap-7.1.3-r0
php7-xml-7.1.3-r0
php7-pdo_odbc-7.1.3-r0
php7-pear-net_socket-1.1.0-r0
php7-7.1.3-r0
php7-session-7.1.3-r0
php7-gd-7.1.3-r0
php7-gettext-7.1.3-r0
php7-mailparse-3.0.2-r0
php7-mcrypt-7.1.3-r0
php7-pdo_dblib-7.1.3-r0
php7-json-7.1.3-r0
php7-mongodb-1.2.8-r0
php7-sysvsem-7.1.3-r0
php7-calendar-7.1.3-r0
php7-iconv-7.1.3-r0
php7-sysvshm-7.1.3-r0
php7-soap-7.1.3-r0
php7-cgi-7.1.3-r0
php7-odbc-7.1.3-r0
php7-pdo_pgsql-7.1.3-r0
php7-zip-7.1.3-r0
php7-pgsql-7.1.3-r0
php7-xdebug-2.5.0-r1
php7-zlib-7.1.3-r0
php7-inotify-2.0.0-r0
php7-couchbase-2.2.3-r1
php7-config-7.1.3-r0
php7-amqp-1.9.0-r0
php7-cassandra-1.2.2-r0
php7-libsodium-1.0.6-r0
php7-pear-net_smtp-1.8.0-r1
php7-bcmath-7.1.3-r0
php7-tidy-7.1.3-r0
php7-zmq-1.1.3-r0
php7-memcached-3.0.3-r0
php7-apcu-5.1.8-r0
php7-sysvmsg-7.1.3-r0
php7-imagick-dev-3.4.3-r1
php7-ftp-7.1.3-r0
php7-ssh2-1.0-r0
php7-pear-net_idna2-0.2.0-r1
php7-pear-auth_sasl-1.1.0-r0
php7-pear-net_smtp-doc-1.8.0-r1
```