# docker-compose-lnmp-swoole

#### 介绍
docker-compose 快速简单部署lnmp 环境，扩展齐全，含有swoole，php和nginx都是使用 alpine。

#### 安装教程

1.  需要安装docker , 安装docker-compose 
2.  创建文件夹www (配置文件可以修改宿主机目录) , 克隆仓库到www 文件夹内
3.  项目根目录下 docker-compose up -d 
4.  项目文件夹 app内含有 数据库和redis，phpinfo得连接测试


#### 使用说明

1.  使用 docker exec -ti {CONTAINER_NAME} /bin/bash 可以查看容器内详细信息
2.  安装配置文件参考 .env 文件 。
3.  安装 docker  教程地址：https://www.runoob.com/docker/centos-docker-install.html
4.  安装docker-compose  教程地址：https://www.runoob.com/docker/docker-compose.html
5.  配置docker 镜像加速器 教程地址：https://cr.console.aliyun.com/cn-shanghai/instances/mirrors
6.  如果有错误得记得私信
