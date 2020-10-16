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
import com.google.gson.JsonParser
import scala.collection.mutable.ArrayBuffer

val jsonparam = """{"RERUNNING":{"nodeName":"test_zhangjiaqi_1","preNodes":[],"prevInterpreters":[],"rerun":"false"},"name":"123123123sdfasdf","path":[{"name":"hdfs://master.xxxx.com:8020/data","path":"hdfs://master.xxxx.com:8020/data"}]}"""

val gson = new Gson()
val parser = new JsonParser();
val p = parser.parse(jsonparam).getAsJsonObject()

//获取单个值
val name = p.get("name")
println(name)

//获取嵌套json对象
val jsonArray = p.getAsJsonArray("path")
val je = jsonArray.get(0).getAsJsonObject
println(je.get("name"))
```