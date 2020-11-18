---
title: docker搭建mysql环境
date: 2020-11-18 10:49:26
tags: 
- docker
categories: Docker
---



```bash
docker pull mysql
docker run -itd --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
docker exec -it mysql bash
```

