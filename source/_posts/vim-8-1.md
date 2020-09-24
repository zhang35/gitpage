---
title: 文本编辑器Vim学习笔记（一）——基础操作
date: 2018-08-01 11:24:30
tags: vim
categories: vim学习笔记 
description: Vim基础指令
---

Vim是一个文本编辑器，遵循程序员的“极懒”原则：能用键盘就不用鼠标，能敲一次键盘解决就绝不敲第二次，手指能在近处就绝不挪远。

![Vim](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTJjM2QwZTZlMWRiNTBhYTgucG5n?x-oss-process=image/format,png)

![加过插件的Vim](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWNlMDM1ZDU5MTMxMTBjOGIucG5n?x-oss-process=image/format,png)

第一次见我哥用，我问他不用鼠标怎么移动光标呢？他给我演示了 行号+G 直接跳到某一行（想象一下不用腾出手挪鼠标，十指保持在键盘上飞舞的效率）。研三做项目时，国科毕业的博士师兄推荐用Vim，买了教材——《Vim 实用技巧》（Practice Vim），自此入坑，至今已三年。Vim虐我千百遍，我待Vim如初恋。

Vim极度高效优雅，比如想要删除一行文字，普通编辑器需要拿鼠标涂黑一整行再按退格，而Vim只需要按“dd”；再比如想要另起一行插入文字，普通编辑器需要拿鼠标找到当前行末尾按回车，而Vim只需要按“o”。

和学Ps一样，下决心学Vim已经不下5次了，这次目标是“从入门到精通”。（入门的话，强烈推荐上述教材，它不仅教了“术”，更是教了“道”）

## 4个模式
vim有4个模式：
- 普通模式 (Normal-mode) ：键盘所有键都成了快捷键，平时最常保持的模式。如同画家作画，更多的是构思、寻找位置，动笔（插入模式）只是最后一个步骤。
- 插入模式 (Insert-mode)：和普通文本编辑器一样，输入什么就是什么。
- 命令模式 (Command-mode)：普通模式下输入“：”即进入，能执行命令行。
- 可视模式 (Visual-mode)：相当于普通文本编辑器下的“涂黑”，先选范围后编辑。

Vim的操作清单如下（手动微笑）：
![vim_cheat_sheet_for_programmers](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWFhMDAxYzI5NDVkMzMzMzYucG5n?x-oss-process=image/format,png)

这些快捷键通过一系列排列组合，能产生强大的效果。

## 两种操作逻辑

### 动作+范围
普通模式下，先输入动作的快捷键，比如d（删除），c（删除并插入），y（复制），p（粘贴）等。

再输入范围，比如w（当前字符后面的单词），aw(当前字符所在的整个单词，包含空格)，iw（当前字符所在的整个单词，不空格），即可形成完整的指令。

组合命令如：

`ciw` ：清除当前单词（不含后边空格），并进入插入模式。

c指change，i指inner，w指word。


### 范围+动作
可视模式下选中的内容，相当于普通文本编辑器的“涂黑”。先选择范围后再按动作键，即可形成指令。

## 常用指令
网上找到的指令速记思维导图：
![图片来自网络](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWY2ZDkzODE5MTgzYzdkYjcucG5n?x-oss-process=image/format,png)

要熟练掌握这些指令，无它，唯有多记多用。