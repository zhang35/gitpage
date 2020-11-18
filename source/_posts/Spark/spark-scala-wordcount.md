---
title: Spark WordCount代码
date: 2020-09-30 10:49:00
tags: spark
categories: spark
description: 
---

```scala
/**
 * Hello world!
 *
 */
import org.apache.spark.SparkContext
import org.apache.spark.SparkConf

object WordCount {
   def main(args: Array[String]) {
      val inputFile =  "file:///E://test.txt"
      val conf = new SparkConf().setAppName("WordCount").setMaster("local")
      val sc = new SparkContext(conf)
      val textFile = sc.textFile(inputFile)
      val wordCount = textFile.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey((a, b) => a + b)
      wordCount.foreach(println)
   }
}
```