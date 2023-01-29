# docker-compose-lnmp-php7&8-swoole

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
* PHP 8.0.13 (FPM) - With MySQLi driver optionally (Uncomment line from php.Dockerfile)
* Nginx 1.22.0
* MariaDB 10.8.3 & mysql:5.7

#### Structure directory
~~~
├── app                      # project
├── db                       # DbLog,data,config
├── nginx                    # nginxLog,config
├── php                      # phpLog,config,dockerFile
├── portainer                # portainer
├── redis                    # redisLog,data,config
.evn                         # docker-compose config
docker-compose.yml           # docker-compose 
~~~

## Configuration
You can also set the following environment variables, for example in the included `.env` file:

## Running
~~~
git clone git@github.com:Se1per/docker-compose-php-lnmp-swoole.git ./www
~~~

php7.4
~~~
cp docker-compose.yml.example74 docker-compose.yml
~~~

php8
~~~
cp docker-compose.yml.example8 docker-compose.yml
~~~

start-up
~~~
cd ./www
docker-compose up -d 
~~~

## port
~~~
php:9000
nginx:80
db:33060
redis:63790
portainer:9030
~~~

## PHP extensions included:
~~~
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
fileinfo
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
memcache
memcached
mongodb
mysqli
mysqlnd
odbc
openssl
pcntl
pcre
PDO
pdo_dblib
pdo_mysql
PDO_ODBC
pdo_pgsql
pdo_sqlite
Phar
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
ssh2
standard
swoole
sysvmsg
sysvsem
sysvshm
tokenizer
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
~~~