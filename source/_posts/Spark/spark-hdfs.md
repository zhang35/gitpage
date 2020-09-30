---
title: Spark操作hdfs
date: 2020-09-30 16:15:00
tags: spark
categories: spark
description: 遍历hdfs目录
---



官方说明：http://hadoop.apache.org/docs/r2.6.1/api/org/apache/hadoop/fs/FileSystem.html

## 遍历hdfs目录文件

```scala
import org.apache.hadoop.conf.Configuration
import org.apache.hadoop.fs.{FileSystem, Path, FileUtil}

val conf = new Configuration()
val hdfs= FileSystem.get(conf)

val savePath = "/"
val path = new Path(savePath)
val allFiles = FileUtil.stat2Paths(hdfs.listStatus(path))
allFiles.foreach(println)
```

```
打印结果：

hdfs://master.xxx.com:8020/2CYS6GRQ3
hdfs://master.xxx.com:8020/31.xlsx
hdfs://master.xxx.com:8020/CanopyPredictParams.json
...
```



## delete操作

```scala
    val hdfs : FileSystem = FileSystem.get(new Configuration)
    val path=new Path(bmNBPath)
    if(hdfs.exists(path)) {
      if(hdfs.isDirectory(path))
        hdfs.delete(path, true) //true代表递归删除，当path是目录时使用
      else hdfs.delete(path,false)  //非递归删除，
    }
```

原型：

```java
public abstract boolean delete(Path f,
             boolean recursive)
                        throws IOException
Delete a file.
Parameters:
f - the path to delete.
recursive - if path is a directory and set to true, the directory is deleted else throws an exception. In case of a file the recursive can be set to either true or false.
Returns:
true if delete is successful else false.
Throws:
IOException
```

## exists操作：检查路径是否存在

原型：

```java
public boolean exists(Path f)
               throws IOException
Check if a path exists. It is highly discouraged to call this method back to back with other getFileStatus(Path) calls, as this will involve multiple redundant RPC calls in HDFS.
Parameters:
f - source path
Returns:
true if the path exists
Throws:
IOException - IO failure
```