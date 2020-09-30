---
title: scala解析json
date: 2020-09-30 10:48:00
tags: scala
categories: scala
description: 使用Gson解析json
---

## Gson解析json

```scala
import com.google.gson.Gson

object App{
  def main(args: Array[String]): Unit = {
	  val jsonparam = """{"tableName":"infoTable"}"""
	  val gson = new Gson()
	  val pGson: java.util.Map[String, String] = gson.fromJson(jsonparam, classOf[java.util.Map[String, String]])
	  val historyTable = pGson.get("tableName")
	  println(historyTable)
  }
}
```

