[TOC]

# docker

docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的 Linux或 Windows 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。



## 主要特性

1. 文件、资源、网络隔离
2. 变更管理、日志记录
3. 写时复制



## docker的安装

官方文档：

- [Mac](https://docs.docker.com/docker-for-mac/install/)
- [Windows](https://docs.docker.com/docker-for-windows/install/)
- [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
- [Debian](https://docs.docker.com/install/linux/docker-ce/debian/)
- [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)
- [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/)
- [其他 Linux 发行版](https://docs.docker.com/install/linux/docker-ce/binaries/)

安装完成后，运行下面的命令，验证是否安装成功：

```bash
docker version
```



启动docker：

```bash
 systemctl start docker
```



检查docker是否安装正确：

```bash
docker run hello-world
```



## 镜像加速

docker中国区镜像加速

可以令pull拉取的时候优先使用中国区镜像加速：

```bash
vi /etc/docker/daemon.json
```

写入如下内容：

```json
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

然后重启docker使配置生效：

```bash
systemctl restart docker
```



## docker的操作

查看正在运行的容器：

```bash
docker ps
```



启动新的容器：

```bash
docker run 容器名
```

docker run相当于执行了两步操作：将镜像放入容器中（docker create）,然后将容器启动，使之变成运行时容器（docker start）。



启动已存在的容器：

```bash
docker start 容器名
```



停止容器：

```bash
docker stop 容器名
```



重启容器：

```bash
docker restart 容器名
```



删除容器，正在运行的容器必须先停止才能删除：

```bash
docker rm 容器名
```



打印容器信息：

```bash
docker logs 容器名
```

参数：

-f 持续打印



## docker-compose

docker-compose工具是一个批量工具，用于运行与管理多个docker容器。

官方文档：[链接](https://docs.docker.com/compose/)



安装：[链接](https://docs.docker.com/compose/install/)



检查docker-compose是否安装正确：

```bash
docker-compose --version
```



使用docker-compose

第一步、新建docker-compose.yml：

```bash
vi docker-compose.yml
```



第二步、写入服务配置：

```bash
version: '3'
services:
  mysql1:
    image: mysql
    environment:
    - MYSQL_ROOT_PASSWORD=zhandi34
    ports:
    - 28002:3306
  mysql2:
    image: mysql
    environment:
    - MYSQL_ROOT_PASSWORD=zhandi34
    ports:
    - 28003:3306
```



第三步、启动docker-compose：

```bash
docker-compose up
```

参数：

+ -d：容器启动后会进入后台



## dockerhub

docker仓库，用于拉取镜像，类似npm

官网： [链接](https://hub.docker.com/)



登录：

```bash
docker login
```



查看docker镜像：

```bash
docker image ls
```



提交镜像：

```bash
docker commit 容器id dockerhub用户名/镜像名:版本号
```



推送镜像到dockerhub：

```bash
docker push dockerhub用户名/镜像名:版本号
```



从dockerhub拉取镜像：

```bash
docker pull dockerhub用户名/镜像名
```

