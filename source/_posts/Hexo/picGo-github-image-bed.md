---
title: PicGo + Typora + Github/Gitee 搭建博客图床
date: 2020-09-27 15:00:00
tags: 
- PicGo 
- Typora 
- Github
- Gitee 
categories: hexo个人博客
description: 本地写博客插图神器
---

使用PicGo图片上传工具，能方便生成Markdown语法需要的图片链接。

图床可选择GitHub或Gitee，这里默认已经创建好了对应用于存储图片的仓库。

## GitHub图床配置

![image-20201118114303368](https://gitee.com/zhang35/Pic/raw/master/blogImg/image-20201118114303368.png)

仓库名：用户名/仓库名

分支名：master

设定Token：Personal access token，获取方式见下图

指定存储路径：上传到仓库中的相对路径

设定自定义域名：https://cdn.jsdelivr.net/gh/用户名/仓库名

（由于国内访问GitHub速度慢，这里使用了jsdelivr加速，只需把上面URL中的用户名/仓库名替换为自己的图片仓库即可）



附：Token获取方式

![image-20201118114452786](https://gitee.com/zhang35/Pic/raw/master/blogImg/image-20201118114452786.png)

新建token时勾选一下repo即可：

![image-20201118114536214](https://gitee.com/zhang35/Pic/raw/master/blogImg/image-20201118114536214.png)



## Gitee图床配置

安装Gitee插件：

![image-20201118115053392](https://gitee.com/zhang35/Pic/raw/master/blogImg/image-20201118115053392.png)

配置类似GitHub图床，只是不需要cdn加速了。

![image-20201118115202188](https://gitee.com/zhang35/Pic/raw/master/blogImg/image-20201118115202188.png)

## Typora配置

最新版本的Typora支持定制上传服务。

在插入图片时设置"上传图片"；

在上传服务设定中，选PicGo，并指定程序安装路径。

点击验证图片上传选项，可以查看是否成功。

![image-20200924142227688](https://cdn.jsdelivr.net/gh/zhang35/Image@master/img/image-20200924142227688.png)



## 常见问题

### Typora图片上传失败

检查PicGo-Server的监听端口是否与Typora要求的一致，默认应为36677：

![image-20200927145016752](https://gitee.com/zhang35/Pic/raw/master/blogImg/image-20200927145016752.png)

### 打开PicGo崩溃

打开PicGo报以下错误：A JavaScript error occurred in the main process

解决方法：重装PicGo，并手动删除`%APPDATA%\picgo`目录

![image-20201118114214399](https://gitee.com/zhang35/Pic/raw/master/blogImg/image-20201118114214399.png)



