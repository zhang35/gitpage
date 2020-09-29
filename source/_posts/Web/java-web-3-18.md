---
title: Java投票网站初版
date: 2018-03-18 18:20:12
tags: java web
categories: Java Web
---

> 搭好[Spring+SpringMVC+Hibernate实现投票/调查问卷网站](https://www.jianshu.com/p/c68fd9fe4ead)的架子后，本想去尝试下其它东西，好在家腾君表示要把它完善下，能真正投入使用。
> 这才发现，还有太多东西要做，还有太多坑没踩。年后至今，三周过去了，终于合作完成了能凑合用的版本。家腾君表示：“吹了一年的牛B，终于能交差了。”


完善后的投票网站，目前效果如下：
### 1. 前端（用户界面）
1.1 打开首页，点击进入投票页面。加了些jQuery美化插件：
![ios视觉差效果（飞机在飞有木有）](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWVkN2QzM2FlODc1YjRjZGQuZ2lm)
![粒子效果](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTI3MjkwOWQwYmU3YjdhNTguZ2lm)

1.2 侧边导航：
![导航栏](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWJlOWQwNWZlYzM2MDBmMGQuZ2lm)

1.3 辅助填表，一键全优：
![一键全优](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWJhMzVlMzZmM2U1YjllNDMuZ2lm)

1.4 表单验证。用户点击提交按钮后，检查答题情况，提示用户：
![提示未完成题目](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWRhODJmYzA5ZmI5ZTZiMzMucG5n?x-oss-process=image/format,png)
![提示用户投票是否有效](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWE1YTEzMTJiNTQyNGEyNjQucG5n?x-oss-process=image/format,png)

1.5 防止重复投票（可由后台管理员开放）。完成一次投票后：
![投票成功](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTUzZDQ5ZTNiOTUyYzQ5YWYucG5n?x-oss-process=image/format,png)
再次投票会提示：
![投票失败](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTQ5NmM4NjMyZDM2ODBhZjIucG5n?x-oss-process=image/format,png)

### 2. 后端（管理员界面）
2.1 输入密码，登录后台：
![登录界面](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWE5M2M3MzY0NjQyZmJkNjIucG5n?x-oss-process=image/format,png)
![管理员界面](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWI1NzVkYTM4MGMwY2E0YTIucG5n?x-oss-process=image/format,png)

2.2 功能区如下：
![功能区](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTFmYmU0ZTEyNDZjNTUwNDkucG5n?x-oss-process=image/format,png)
- 每2s实时更新投票情况。
- “开放投票”按钮会刷新本轮投票情况，去除“无法重复投票”状态；
- “无限投票”开关打开时，永不限制重复投票；
- “下载文件”按钮会将结果导出为word文档，压缩为zip文件，提供下载；
- “过滤废票”开关打开后，会按规则去除无效票，改变统计结果。

2.3 点击人名，查看结果。如果打开“过滤废票”开关，会显示去除废票后的结果：
![查看详细结果](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWI1ZGFlY2M5MjAyZGMwMDMucG5n?x-oss-process=image/format,png)

2.4 下载结果。如果打开“过滤废票”开关，会得到去除废票后的结果：
![下载得到“测评结果.zip”文件](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWZmYTFmOTg5YzViYzBlYmYucG5n?x-oss-process=image/format,png)
![解压得到按部门分类目录结构](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTVjYzQ3ZjNiMjMxNzljOTkucG5n?x-oss-process=image/format,png)
![得到自动生成的word文件xxx.doc](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTAxYTljMzg4NDY4ZDM5Y2IucG5n?x-oss-process=image/format,png)

以上。

几乎全是“面向搜索引擎”的编程。后面会继续完善，慢慢总结所用知识。
项目源码：[https://github.com/zhang35/QuizWeb.git](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fzhang35%2FQuizWeb.git)