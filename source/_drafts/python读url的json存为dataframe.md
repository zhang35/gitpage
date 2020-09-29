---
title: pyspark获取url返回的json存为dataframe
tags: pyspark
---



```python
import json
import requests
from pyspark.shell import spark, sc

r = requests.get('https://trajectoryanalysis.azurewebsites.net/lastappeared')
dataJsonList = r.json()

rdd = sc.parallelize(dataJsonList).map(lambda x: json.dumps(x))
df = spark.read.json(rdd)

df.show()
```



打印结果：

```
+-------------------+----+----+---------+
|  lastmodified_time| lat|long|object_id|
+-------------------+----+----+---------+
|2020-09-24 17:11:00|1.25| 2.0|       20|
|2020-09-24 17:11:00|1.25| 2.0|       21|
|2020-09-24 17:11:00|1.25| 2.0|       23|
|2020-09-23 16:11:00| 1.1| 2.0|       14|
|2020-09-24 17:11:00|1.25| 2.0|       30|
+-------------------+----+----+---------+
```



df.collect()返回String List 

