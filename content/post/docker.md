---
title: Docker
date: 2018-09-05 10:14:14
tags:
- docker
---



# OS

CentOS 7



本文的设定是使用Centos7.4版本，内核是3.10.0。



# 更新YUM源信息

```bash
yum makecache
```



Docker CE版本



2018年9月最新版本

18.06.1-ce

见[Wiki](https://en.wikipedia.org/wiki/Docker_(software))





添加源

鉴于国内网络问题，强烈建议使用国内源执行下面的命令添加 yum 软件源：

```
sudo yum-config-manager \
    --add-repo \
    https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo
```





#### 卸载旧版本Docker

```bash
sudo yum remove docker \
           docker-common \
           docker-selinux \
           docker-engine
```





```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
```



  关闭防火墙

```bash
systemctl stop firewalld.service #停止
systemctl disable firewalld.service #禁用
```



# 安装

yum install docker-ce





## 镜像加速

鉴于国内网络问题，后续拉取 Docker 镜像十分缓慢，强烈建议安装 Docker 之后配置 国内镜像加速。



针对Docker客户端版本大于 1.10.0 的用户

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

```bash
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://8z9d5u5v.mirror.aliyuncs.com"]
}
EOF

sudo systemctl daemon-reload

sudo systemctl restart docker
```



#### 建立 docker 用户组

默认情况下，`docker` 命令会使用 [Unix socket](https://link.jianshu.com?t=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FUnix_domain_socket) 与 Docker 引擎通讯。而只有 `root`用户和 `docker` 组的用户才可以访问 Docker 引擎的 Unix socket。出于安全考虑，一般 Linux 系统上不会直接使用 `root` 用户。因此，更好地做法是将需要使用 `docker` 的用户加入 `docker` 用户组。

建立 docker 组：

```bash
sudo groupadd docker
```

将当前用户加入 docker 组：

```bash
sudo usermod -aG docker $USER
```

退出当前终端并重新登录，进行如下测试。











[参考1](https://my.oschina.net/shyloveliyi/blog/1616025)

[参考2](http://www.runoob.com/docker/centos-docker-install.html)

[阿里安装教程](https://yq.aliyun.com/articles/110806?spm=5176.8351553.0.0.1a6c199116s32Q)





# YAML文件格式



[官方](https://docs.docker.com/docker-cloud/apps/stack-yaml-reference/)



# 数据卷



Union File System（联合文件系统）



Volume的概念

是一个目录

内部Volume: /``var``/``lib``/``docker``/``volumes``/

自己指定路径



数据卷是一个可供一个或多个容器使用的特殊目录，它绕过 UFS



MySQL的[Dockfile](https://github.com/docker-library/mysql/blob/9d1f62552b5dcf25d3102f14eb82b579ce9f4a26/5.7/Dockerfile)有这样一个命令

```dockerfile
VOLUME /var/lib/mysql
```

导致使用了内部Volume。





[入门](https://yeasy.gitbooks.io/docker_practice/data_management/volume.html)



挂载一个主机目录作为数据卷

```bash
sudo docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py
```

上面的命令加载主机的 `/src/webapp` 目录到容器的 `/opt/webapp` 目录。



MySQL



```dockerfile
version: '2'

networks:
  default:
    external:
      name: szrd_bridge

services:

    szrd_mysql:
        image: mysql:5.7.17

        volumes:
        - my-vol:/var/lib/mysql
        
        environment:
        - MYSQL_ROOT_PASSWORD=abc123
        
        restart: always
        
        ports:
        - "6000:3306"

volumes:
  my-vol:
    driver: local
```



my-vol是一个内部Volume。



从服务器将内部Volume拷贝过来。

```dockerfile
version: '2'

networks:
  default:
    external:
      name: szrd_bridge

services:

    szrd_mysql:
        image: mysql:5.7.17

        volumes:
        - my-vol:/var/lib/mysql
        
        environment:
        - MYSQL_ROOT_PASSWORD=abc123
        
        restart: always
        
        ports:
        - "6000:3306"
  
volumes:
  my-vol:
    driver: local
    driver_opts:
      o: bind
      type: none
      device: /home/yanghaibin/redmine/vol/_data
```





