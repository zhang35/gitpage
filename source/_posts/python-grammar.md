---
title: python-grammar
date: 2020-09-24 09:52:21
tags: python
description: 记录python3基础语法
categories: python
---



## 列表推导式生成list

```python
# in后面跟其他可迭代对象,如字符串
list_c = [7 * c for c in "python"]
print(list_c) 

# 带if条件语句的列表推导式
list_d = [d for d in range(6) if d % 2 != 0]
print(list_d) 

# 多个for循环
list_e = [(e, f * f) for e in range(3) for f in range(5, 15, 5)]
print(list_e) 

# 嵌套列表推导式,多个并列条件
list_g = [[x for x in range(g - 3, g)] for g in range(22) if g % 3 == 0 and g != 0]
print(list_g)
```

运行结果:

```python
['ppppppp', 'yyyyyyy', 'ttttttt', 'hhhhhhh', 'ooooooo', 'nnnnnnn']
[1, 3, 5]
[(0, 25), (0, 100), (1, 25), (1, 100), (2, 25), (2, 100)]
[[0, 1, 2], [3, 4, 5], [6, 7, 8], [9, 10, 11], [12, 13, 14], [15, 16, 17], [18, 19, 20]]
```

参考：[Python列表推导式_weixin_43790276的博客-CSDN博客](https://blog.csdn.net/weixin_43790276/article/details/90247423)

## Dict操作

### 找出最大值

```python
sampleDict = {'Ritika': 5, 'Sam': 27, 'John': 12, 'Sachin': 14, 'Mark': 19, 'Shaun' : 27}
itemMaxValue = max(sampleDict.items(), key=lambda x: x[1])
maxValue = max(sampleDict.values())
print(itemMaxValue[0], maxValue)

# output:
# Sam 27
```

