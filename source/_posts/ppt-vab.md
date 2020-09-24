---
title: 如何用vba做答题、抽点类ppt
date: 2018-11-24 11:13:21
tags: 
- ppt 
- excel 
- vba 
categories: vba编程
description: 为了给ppt添加复杂功能，需要后台插入vba代码。下面整理一下为ppt插入、关联vba代码的基本流程，以及一些Bug的解决方法。
---
首先严重声明一下，我不是ppt高手，酷炫动画、美化之类一概不会，我只会写点代码，汇报ppt做得要多丑有多丑。

16年底，有幸拿到一份答题ppt模板，接手做了“XX知识问答竞赛”ppt，包含选题答题、倒计时、随机抽点等功能，初次接触到了VBA（Visual Basic for Applications）编程。

于是自那以后包办了单位各种答题类ppt……最近利用工作间隙又干了一票，感到是时候总结一下，把这项光荣而艰巨的任务传递下去了。

![随机抽点](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWY2MzhmMzUxZDI2NGRlZDcuZ2lm)

![选题答题，加减分](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWNlNDc0OWMzZDA2NWExMmYuZ2lm)

![倒计时，并播放提示音](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWNlYmRkYTU4MzNiZDZiYWYuZ2lm)

为了给ppt添加复杂功能，需要后台插入vba代码。下面整理一下为ppt插入、关联vba代码的基本流程，以及一些Bug的解决方法。

开发环境：Win10 x64，office 2016。

## 准备工作
### 显示“开发工具”
在菜单栏显示“开发工具”，方便后续开发。
打开ppt，点 文件->选项->自定义功能区，勾选“开发工具”。
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTIwMTM4NTliMzNiMTQxZTMucG5n?x-oss-process=image/format,png)

![出现“开发工具”菜单](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWQzZTA0MjFiNzI1OGIzNGMucG5n?x-oss-process=image/format,png)

### 选择引用库
当需要读写Excel时，需勾选引用库。

![点开发工具->工具->引用，勾选“Microsoft Excel 16.0 Object Library”](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTk2MTFjNTJhYzk3ZjliYzAucG5n?x-oss-process=image/format,png)

**注意：随office版本不同，16.0可能会变成12.0等，更换版本时（比如拿office 2010做的拷到office 2016的电脑上用）需要正确勾选。**

## 基本流程
### 插入形状
![新建一页ppt，插入一个形状](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTY5NTU1NmJmMjY5Mzk5ZDAucG5n?x-oss-process=image/format,png)

### 插入按钮
![点菜单栏->开发工具->“命令按钮”，在页面上拖动，插入按钮](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTMxMzRhY2U3ODU4YjYwOWIucG5n?x-oss-process=image/format,png)

### 打开选择窗格，为对象命名（很重要！）
![点菜单栏->开始->选择->选择窗格，打开对象选择窗口](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTBmYWQzNjMwZTc4NmQzZmEucG5n?x-oss-process=image/format,png)

![双击将矩形对象名，改名为shape_text，这就是VBA中关联的形状名](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LThmMzRhN2VjZGRhZGJmODEucG5n?x-oss-process=image/format,png)

### 写代码
![双击按钮，或点菜单栏->开发工具->查看代码，进入开发页面](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTM1ZDIzOWNlODRlMWMzNDYucG5n?x-oss-process=image/format,png)

![写入如下代码，功能是在形状上显示一行文字](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWIyMDY0MTlkNjIyM2QzYjkucG5n?x-oss-process=image/format,png)

```VB
Private Sub CommandButton1_Click()
Shapes("shape_text").TextFrame2.TextRange.Text = "你好，VBA！"
End Sub
```

### 关联代码
也可以为任何形状关联一段代码。需把代码片段声明中的“Private”关键字去掉，比如：
```VB
Sub Show()
Shapes("shape_text").TextFrame2.TextRange.Text = "你好，VBA！"
End Sub
```

然后回到ppt页面，为形状关联代码：
![选中形状，点菜单栏->插入->动作->运行宏](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWRjZWFkZGMzZTJkM2Q5ZDcucG5n?x-oss-process=image/format,png)


### 运行代码
方式一，播放ppt运行代码：
![点击菜单栏最左侧按钮，返回ppt页面](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWZiNWFhZmY3MmRjZDlmYmIucG5n?x-oss-process=image/format,png)

![播放ppt，点击按钮，出现文字](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWFkYTlmMTEzNjc1OTg2YjQucG5n?x-oss-process=image/format,png)

方式二，在开发页面直接运行代码（常用于调试）：
![将光标放到希望运行的函数内，点菜单栏运行按钮](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWQyNGUxNDMwM2RkOTM3MWYucG5n?x-oss-process=image/format,png)

以上是ppt vba开发基本流程，下面附上部分代码。

## 部分关键代码
### 随机抽点：
```VB
Private Declare PtrSafe Sub Sleep Lib "kernel32" (ByVal dwMilliseconds As Long)

'开始
Sub RandomStart()
    F = 0
    Do While True
        If F = 1 Then Exit Do
        currentQuestionNum = Int(num3_5 * Rnd)
        Shapes("lable_text").TextFrame2.TextRange.Text = question3_5(currentQuestionNum, 0)
        Sleep 20
        DoEvents
    Loop
End Sub

'结束
Sub RandomStop()
F = 1
Shapes("shape_answer").Visible = msoTrue
End Sub
```

### 选题答题：
```VB
Sub chooseQuestion20(i As Integer)
    '题号消失
    Shapes(questionShape20(i)).TextFrame2.TextRange.Text = "" 
    '出现"显示答案"按钮
    Shapes("shape_answer").Visible = msoTrue
    '显示题目
    currentQuestionNum = i
    Shapes("lable_text").TextFrame2.TextRange.Text = question3_1(currentQuestionNum, 0)
End Sub
```
### 显示图片：
```VB
Shapes("pic1").Fill.UserPicture (ActivePresentation.Path & "\照片库\1.jpg")
```

## 各种疑难杂症
遇到过各种神奇的问题，网上对ppt vba方面问题解答较少，有些解决起来费了些功夫。
### 无法正常读取Excel
参考上文 “准备工作” “选择引用库” 。

### 出现“缺少Sub或Function”错误

这是在office2016上开发后，换到office2007电脑上运行报的错。

解决方法：尽量保持office版本一致，建议使用2010以上版本。

### 64位系统下，出现“类型不匹配”错误
从32位系统迁移到64位系统后，运行倒计时函数CreateTimer：
```VB
Private Declare PtrSafe Function SetTimer Lib "user32" (ByVal hWnd As _
        Long, ByVal nIDEvent As Long, ByVal uElapse As Long, _
        ByVal lpTimerFunc As Long) As Long

Private Function CreateTimer(ByVal Interval As Long) As Long
    ' 建立一个时间间隔为Interval微秒的定时器
    Dim tID As Long
    tID = SetTimer(0, 0, Interval, AddressOf TimerProc) '运行到此处出错
    CreateTimer = tID
End Function

Private Sub TimerProc(ByVal hWnd As Long, ByVal Msg As Long, ByVal idEvent As Long, ByVal dwTime As Long)
    ' 此处放入要执行的代码
    CounterNumber
End Sub
```
出现以下错误：
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LWQ0MThiNDA3ZjcwOWQzMjMucG5n?x-oss-process=image/format,png)

解决方法：将SetTimer声明的最后一个参数类型改为LongPtr，即指针类型即可。
```VB
Private Declare PtrSafe Function SetTimer Lib "user32" (ByVal hWnd As _
        Long, ByVal nIDEvent As Long, ByVal uElapse As Long, _
        ByVal lpTimerFunc As LongPtr) As Long
```

## 结语
没有系统学过VB，但由于是类C语言，在有源码的支撑下，比葫芦画瓢拿来用并不费力，当时从最初接触到完成开发只用了一周时间。

但至今仍有许多未解决的疑惑，例如：

1. 如何将被点击形状的名称作为参数，使其被VBA代码捕获。目前为每个形状关联不同的函数，50个形状就要写50个函数、改50个名称、关联50次……；
2. 如何随页面载入自动运行某段函数。目前采用手动点击按钮的方式初始化。

上述问题可能在ppt vba中无解，也可能有更好的解决方法，欢迎交流。