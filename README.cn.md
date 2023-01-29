# docker-compose-lnmp-php74-php8-swoole

#### 介绍
docker-compose 快速简单部署 lnmp 环境，扩展齐全，含有 swoole，php8和nginx dockerFile和镜像都使用alpine。

#### 默认安装版本
1. php 8.0.13 && 7.4
2. Swoole 4.8.11
3. nginx 1.22.0
4. mariadb:10.8.3 && mysql:5.7(可选)
5. redis 6.0.7
6. portainer latest

#### 结构目录
~~~
├── app                      # 项目相关
├── db                       # 数据库日志,存储,配置
├── nginx                    # nginx日志,配置
├── php                      # php日志,配置,dockerFile
├── portainer                # 管理docker开源面板
├── redis                    # redis日志,存储,配置
.evn                         # docker-compose 环境配置文件
docker-compose.yml           # docker-compose 文件
~~~

#### 安装教程

1. 需要安装docker , 安装docker-compose
2. 在liunx 下 ./ 创建文件夹www, 克隆仓库到www 文件夹内
~~~
克隆仓库
# 注意克隆仓库得路径是 /www 
git clone git@github.com:Se1per/docker-compose-php-lnmp-swoole.git ./www

~~~
3. 项目根目录下
~~~
第一步:
php 7 版本执行: cp docker-compose.yml.example74 docker-compose.yml
php 8 版本执行: cp docker-compose.yml.example8 docker-compose.yml
~~~
~~~
docker-compose up -d 
~~~
5. 项目文件夹 app内含有 数据库和redis，phpinfo得连接测试

7. 注意,需要修改的请自行根据dockerhub可以进行版本更替,具体修改和查看.env 需要先创建一级目录www,不然的话默认的配置文件映射会失效,加速使用阿里云,测试都是用阿里云节点,如果网络不好可以试试清华的节点

7.新增portainer 自己直接访问地址 9030 端口进行访问

#### 使用说明

1. 使用 docker exec -ti {CONTAINER_NAME} /bin/bash 可以查看容器内详细信息
2. 安装配置文件参考 .env 文件 。自定义配置版本选择,或者自己去dockerhub 找对应镜像
3. 安装 docker 教程地址：https://www.runoob.com/docker/centos-docker-install.html
4. 安装docker-compose  教程地址：https://www.runoob.com/docker/docker-compose.html
5. 配置docker 镜像加速器 教程地址：https://cr.console.aliyun.com/cn-shanghai/instances/mirrors
6. 如果有错误得记得私信
7. 如果部分得出现evn环境无法读取运行得建议重装一下docker-compose

#### 补充说明
端口定义默认都是在 yml 文件中,以下列出默认得端口映射关系
~~~
php:9000 (内部)
nginx:80
db:33060
redis:63790
portainer:9030
~~~

#### 扩展列表
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