---
title: scala try-Catch
date: 2020-09-30 16:48:00
tags: scala
categories: scala
description: scala Try-Catch
---

## try-Catch

```scala

​```
try {
    // ...
} catch {
    case ex: Exception => {
        ex.printStackTrace() // 打印到标准err
        System.err.println("exception===>: ...")  // 打印到标准err
    }
}
​```

​```
		try {
			val a = 1 / 0
		} catch {
			case ex: Exception => {
				println("bug")
			}
		}
​```

```

