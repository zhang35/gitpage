---
title: 量化考评网站初版
date: 2018-08-29 11:27:12
tags: 量化考评 web 购物车 成长曲线 评语 angular.js gulp bower jade 
categories: 前端开发
description: 量化考评网站前端部分。用到angular.js、gulp、bower、jade等技术,实现绩效填写、购物车增减条目、成长曲线、评语等功能
---

在李成海大神搭的架子下，完成了量化考评网站前端部分。用到angular.js、gulp、bower、jade等，感谢成海指导。

18年7月31日，初版于单位内网上线，功能如下。

## 用户入口

### 登录页面
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWIwYzJjZDA4NDljNjYyZDUuZ2lm)

### 考评页面
#### 选择部门、日期
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWY3NTZiZDBmMjhlZDRlMzguZ2lm)

#### 填写一日工作
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWQwNDI3ZDE4YjRjZmUzNTcuZ2lm)

- 类似**购物车**功能，增减或直接修改数目，可实时看到得分结果。

- 当数目输入不合法时（字母，负数等），自动替换为0。

- 加分数目和减分数目均为0时，得分将被清空，不做统计。

#### 快速导航按类别定位

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWJlYzM4MDQ3NDViY2I3YzguZ2lm)

自动按类别生成导航菜单。

#### 提交今日工作
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTg3ZTcwNzU3ZDI3YjM1M2QuZ2lm)

显示今日总得分，提交后显示明细。

#### 防止重复提交
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTBjOWQyNzVlYzA2MjYwYWQuZ2lm)
显示“今日已提交！”，提交按钮变灰。

工作记录限制每天提交一次，一经提交不可更改。

### 绩效搜索页面

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWUyMGI4YmFhYmI4NWViMTMuZ2lm)

查看个人绩效，可按得分类型、评语类型、起止日期等筛选。

### 成长曲线页面
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTEzNjU3MjgzNWEzMGM0NjMuZ2lm)

查看个人成长曲线，由每日得分构成。可按部门、起止日期筛选。

## 领导入口
### 登录页面
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWE2NTM3MTgyNzI5MWE5YzMuZ2lm)

### 绩效搜索页面
#### 检索人员绩效
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWM1MDIzZDMzNDU2NTcyN2EuZ2lm)

检索所属人员绩效，可按部门、姓名、得分类型、评语类型、起止日期等筛选。

#### 填写评语
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTliYzAwYTE3OTM2MWI3YTQuZ2lm)

领导可对某条绩效添加评语。普通用户将能在绩效搜索页面看到评语。

### 成长曲线页面
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTM4NDQ3N2ZhZWRjMDNkNzcuZ2lm)

查看所属人员成长曲线，可按部门、姓名、起止日期等筛选。

## 结语 
网站后端由成海开发，使用springboot脚手架，效率极高地提供了数据增删查改的接口，这是后面需要学习的东西。

借此体验了前后端分离的合作开发模式：在阿里云服务器上搭建测试网站和数据库服务，在GitHub上共同管理代码。

源码可在我的GitHub上找到：https://github.com/zhang35