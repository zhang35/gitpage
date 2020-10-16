---
title: Centos7 离线安装Docker
date: 2020-10-12 10:29:26
tags: 
- centos7 
- offline
- docker
categories: Docker
description: 简单二进制安装
---

参考： [https://docs.docker.com/engine/install/binaries/](https://docs.docker.com/engine/install/binaries/)


测试环境如下，centos系统版本为3.10.0-327.el7.x86_64：
```
$ uname -r
3.10.0-327.el7.x86_64
```

### 下载二进制安装包
[https://download.docker.com/linux/static/stable/x86_64/](https://download.docker.com/linux/static/stable/x86_64/) 

这里下载了`docker-17.03.0-ce.tgz`，拷贝到centos里。

注意不能使用docker-18.06版本，docker版本过高而linux版本过低，会报错：

```bash
docker: Error response from daemon: OCI runtime create failed: container_linux.go:348: starting container process caused "process_linux.go:297: copying bootstrap data to pipe caused \"write init-p: broken pipe\"": unknown.

```

### 解压二进制包

`$ tar xzvf docker-17.03.0-ce.tgz`

### 将二进制文件移动到/usr/bin/中：

`$ sudo cp docker/* /usr/bin/`

### 开启守护进程

`$ sudo dockerd &`

### 验证安装

`$ sudo docker run hello-world`

成功打印：

```bash
Hello from Docker!
This message shows that your installation appears to be working correctly.
```