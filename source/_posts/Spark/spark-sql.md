---
title: Spark读写数据
date: 2020-09-30 10:48:00
tags: spark
categories: spark
description: Spark读Mysql、CSV数据源
---

## 读Mysql作为DataFrame

```scala
package Mysql
import java.util.Properties

import org.apache.spark.sql.DataFrame
import org.apache.spark.sql.hive.HiveContext
import org.apache.spark.{SparkConf, SparkContext}

object SparkSQL1 {

  def main(args: Array[String]): Unit = {

    val conf = new SparkConf().setAppName(s"${this.getClass.getSimpleName}").setMaster("local")
    val sc = new SparkContext(conf)
    val sqlContext = new HiveContext(sc)
	
    //实例代码
    val properties = new Properties()
	properties.put("user","xxx")
	properties.put("password","xxx")
	val url = "jdbc:mysql://192.168.11.26:3306/test_db"
	val df: DataFrame = sqlc.read.jdbc(url,"info_table",properties)
	df.show()
  }
}
```

## 读CSV作为DataFrame

```scala
	val data: DataFrame = sqlc.read.format("com.databricks.spark.csv")
			.option("header", "true") //在csv第一行有属性"true"，没有就是"false"
			.option("inferSchema", true.toString) //这是自动推断属性列的数据类型
			.load("E:/test.csv")
	data.show
```

