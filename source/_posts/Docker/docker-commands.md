---
title: docker基础命令及手动保存、装载镜像方法
date: 2020-10-12 10:49:26
- docker
categories: Docker
description: 基础指令
---

### 基础命令
获取镜像：`docker pull [OPTIONS] NAME[:TAG|@DIGEST]`
查看镜像：`docker images`
运行镜像：`docker run [image]`
停止镜像：`docker stop [image]`
删除镜像：`docker image rm [image]`

或`docker rmi [image]`

查看进程：`docker ps -a`
删除容器：`docker rm [container]`
查看容器ip：`docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' \ [container]`
（bridge模式下，此ip为docker内网ip，本地主机无法访问）
查看log：`docker logs [container]`

注：`[image]`、`[container]`既可以是id，可以是name。

创建volume:`$ docker volume create my-vol`
查看volume：`$ docker volume ls`
查询volume:`$ docker volume inspect my-vol`
删除volume:`$ docker volume rm my-vol`



拷贝文件：`  docker cp 宿主机文件路径  镜像名称:镜像中文件存放路径`

拷贝文件夹：`$ docker cp docker-offline/. centos:/root/`

### 手动保存/装载image
离线安装镜像，在联网主机上：
`$ docker pull hello-world`
`$ docker save --output hello-world.tar {your image name or ID}`

在离线主机上：
`$docker load --input hello-world.tar`