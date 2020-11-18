---
title: pyspark wordcount
date: 2020-09-30 10:48:00
tags: spark
categories: spark
description: 
---

## pyspark环境配置

参考：https://www.cnblogs.com/TTyb/p/9546265.html

在命令提示符里输入pyspark验证，应该打印如下内容：

![image-20201019175846163](https://cdn.jsdelivr.net/gh/zhang35/Image@master/img/image-20201019175846163.png)

如果不成功，报如下错误，说明spark环境变量有问题。

```
Failed to find Spark jars directory.
You need to build Spark before running this program.
```

windows下路径为：

```bash
D:\spark-3.0.1-bin-hadoop2.7\bin
```

也就是pyspark.cmd所在的目录一定要被包含到环境变量中。



## 测试程序

建立test.txt内容如下:

```
abc
def
abc
```

测试程序如下：

```python
from pyspark import SparkContext
sc = SparkContext("local", "first app")
logFile = "file:///E://test.txt"
logData = sc.textFile(logFile).cache()
numAs = logData.filter(lambda s: 'a' in s).count()
numBs = logData.filter(lambda s: 'b' in s).count()
print ("Lines with a: %i, lines with b: %i" % (numAs, numBs))
```

打印结果：

```
Lines with a: 2, lines with b: 2
```

