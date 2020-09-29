---
title: sql-grammar
date: 2020-09-29 10:53:21
tags: sql
description: 记录sql基础语法
categories: SQL
---


## 字符串截取
substring(str, pos); 
substring(str, pos, len)
上面的表达式可以省略掉第三个参数，如：
substring(`station`,instr(`station`, '号线')+2)
能正常运行。

## 字符串合并
concat(str1, str2,…)
例如：concat(LINE_NO,'号线',ST_NAME)



## 日期比较

select * from trajectory_copy
WHERE DATE_SUB(lastmodified_time, INTERVAL 8 HOUR) >= DATE_SUB(NOW(), INTERVAL 2 MINUTE)



## 注意

注意时间格式的区别：
yyyy/MM/dd HH:mm:ss
yyyy-MM-dd HH:mm:ss    
防止出现意料之外的错误。

