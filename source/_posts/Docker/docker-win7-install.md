---
title: Win7安装Docker
date: 2020-10-12 10:20:26
tags: 
- Win7
- docker
categories: Docker
---

win7系统下，一开始下载了docker desktop，发现安装失败：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201009110101208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nMzU=,size_16,color_FFFFFF,t_70#pic_center)
因为docker desktop要求win10系统，老版本的只能安装docker toolbox。

下载toolbox：[https://github.com/docker/toolbox/releases](https://github.com/docker/toolbox/releases)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201009110845854.png#pic_center)
安装完成后，得到一个快捷方式，直接运行报错：windows 正在查找bash.exe。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201009124951321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nMzU=,size_16,color_FFFFFF,t_70#pic_center)

解决方法：手动配置git路径：

```bash
D:\Git\bin\bash.exe --login -i "C:\Program Files\Docker Toolbox\start.sh"
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201009125216762.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nMzU=,size_16,color_FFFFFF,t_70#pic_center)
再次运行，仍然有错误：

```bash
No default Boot2Docker ISO found locally…
error with pre-create check
```

解决方法：下载Boot2Docker ：
[https://github.com/boot2docker/boot2docker/releases](https://github.com/boot2docker/boot2docker/releases)
到`%HOMEPATH%.docker\machine\cache`

我的是：`C:\Users\Administrator\.docker\machine\cache`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201009133348451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nMzU=,size_16,color_FFFFFF,t_70#pic_center)
输入` $ docker run hello-world`，看到如下信息，表明正常运行：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201009133435790.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nMzU=,size_16,color_FFFFFF,t_70#pic_center)

上面的docker tool box 快捷方式打开的窗口不好用，不能复制粘贴。

解决方法：直接在docker安装目录下启动star.sh即可：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201009142804828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nMzU=,size_16,color_FFFFFF,t_70#pic_center)