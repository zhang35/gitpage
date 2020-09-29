---
title: 文本编辑器Vim学习笔记（二）——移动、跳转
date: 2018-08-06 11:24:30
tags: vim
categories: vim学习笔记 
description: Vim移动、跳转
---

为了摆脱鼠标，高效准确的移动光标是首要的。下面总结移动指令。

## 上下左右

| 位置 | 按键 | 备注 |
| ---- | ---- | ---- |
| 上   | k    |      |
| 下   | j    |      |
| 左   | h    |      |
| 右   | i    |      |

可以前加数字，表示移动的倍数。例如，"10j"表示下移动10行。

## 行
| 位置               | 按键              | 备注               |
| ------------------ | ----------------- | ------------------ |
| 首行               | gg                |                    |
| 末行               | G                 |                    |
| 第{n}行            | {n}G、{n}gg、:{n} | ctrl-g查看当前位置 |
| 文件{n}%处的行     | {n}%              |                    |
| 行首               | 0                 |                    |
| 行尾               | $                 |                    |
| 行内第一个非空字符 | -                 |                    |

## 屏幕滚动
| 位置               | 按键   | 备注   |
| ------------------ | ------ | ------ |
| 下一页             | ctrl-f |        |
| 上一页             | ctrl-b |        |
| 下半页             | ctrl-d |        |
| 上半页             | ctrl-u |        |
| 下一行             | ctrl-e |        |
| 上一行             | ctrl-y |        |
| 当前页顶部         | H      | Head   |
| 当前页中部         | M      | Medium |
| 当前页底部         | L      | Last   |
| 当前行滚到屏幕顶部 | zt     |        |
| 当前行滚到屏幕底部 | zb     |        |
| 当前行滚到屏幕中部 | zz     |        |

## 单词
| 位置               | 按键 | 备注 |
| ------------------ | ---- | ---- |
| 下一词首           | w    |      |
| 下一词首（含符号） | W    |      |
| 上一词首           | b    |      |
| 上一词首（含符号） | B    |      |
| 下一词尾           | e    |      |
| 下一词尾（含符号） | E    |      |
| 上一词尾           | ge   |      |
| 下一词尾           | gE   |      |

## 历史记录
| 位置     | 按键   |
| -------- | ------ |
| 上次位置 | ctrl-o |
| 下次位置 | ctrl-i |

## 搜索
搜索是最快速的跳转。

|位置|按键|备注|下一个|上一个
|-|-|-|-|-|
|下个{char}|f{char}||;|,|
|上个{char}|F{char}||;|,|
|下个{char}前|t{char}||;|,|
|上个{char}前|T{char}||;|,|
|下个{string}|/{string}|正则表达式|n|N|
|上个{string}|?{string}|正则表达式|n|N|

## mark
设置标签：m{mark}
查看标签：:marks
跳转到标签：`{mark}

Vim常用自动创建的位置标志有：
| 位置             | 标签名 | 备注 |
| ---------------- | ------ | ---- |
| 上次跳转位置     | `      |      |
| 上次修改位置     | .      |      |
| 上次修改位置开头 | [      |      |
| 上次修改位结尾   | ]      |      |

## 跳转
| 位置         | 按键   | 例子 |
| ------------ | ------ | ---- |
| 关键字定义处 | ctrl+] |      |
| 括号匹配处   | %      |      |


## 折叠

Vim有 6 种折叠模式：
- manual           手工定义折叠
- indent             更多的缩进表示更高级别的折叠
- expr                用表达式来定义折叠
- syntax             用语法高亮来定义折叠
- diff                  对没有更改的文本进行折叠
- marker            对文中的标志折叠

在.vimrc里增加set fdm=indent，即开启自动折叠。

常用操作为：

| 操作         | 按键 | 例子        |
| ------------ | ---- | ----------- |
| 关闭所有折叠 | zm   |             |
| 展开所有折叠 | zr   |             |
| 创建折叠     | zf   | zf% 、zf56G |
| 关闭折叠     | zc   |             |
| 嵌套关闭折叠 | zC   |             |
| 展开折叠     | zo   |             |
| 嵌套展开折叠 | zO   |             |
| 下一个折叠   | zj   |             |
| 上一个折叠   | zk   |             |

## EasyMotion插件
Vim官网上评级为life changing的插件，能实现跨越多行的精准跳转。

使用方法：在原有的跳转指令前加{leader}{leader}键，{leader}默认为反斜杠`\`。

例如，输入`\\w`,效果如下：
![初始位置](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWMyNzFiNTQyZjJjMjYyODkucG5n?x-oss-process=image/format,png)
![指令效果](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWUyZTg4YTk0MDU4OTJmYTIucG5n?x-oss-process=image/format,png)

输入位置对应的字母即可跳转过去。太帅了。

## 小结
总结了Vim常用移动和跳转指令，及Easymotion插件。