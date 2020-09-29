---
title: C双向冒泡排序算法
date: 2018-11-24 11:21:10
tags:
- bubble sort
- c
- both sides
description: 上浮和下沉过程交替的冒泡排序算法。
categories: 算法
---

同事考研遇到的数据结构题：

> 题目：冒泡排序算法是把大的元素向上移（气泡的上浮），也可以把小的元素向下移（气泡的下沉），请给出上浮和下沉过程交替的冒泡排序算法。

为了减少重复代码，设置了变量step在1、-1间变化，来控制正向或反向冒泡。

```
//
//  main.cpp
//  双向冒泡算法
//  Created by 家齐 on 2018/11/10.
//
#include <stdio.h>
void PrintArray(int *a, int n){
    for (int i=0; i<n; i++){
        printf("%d ", a[i]);
    }
    printf("\n");
}

void BubbleSort(int *array, int n){
    int i, j, k, temp, step;
    i = 0;
    step = 1;  //控制方向
    for (k=n-1; k>0; k--){
        for (j=0; j<k ; j++){
            if (array[i+1]<array[i]){
                temp = array[i];
                array[i] = array[i+1];
                array[i+1] = temp;
            }
            printf("%d\n",i);
            i += step;
            PrintArray(array, n);
        }
        
        printf("\n");
        //i回退两个位置
        i -= step;
        i -= step;
        
        step *= -1; //反向
    }
}

int main(int argc, const char * argv[]) {
    int a[5] = {5, 4, 3, 2, 1};
    BubbleSort(a, 5);
    return 0;
}
```
打印结果如下：
```
4 5 3 2 1 
4 3 5 2 1 
4 3 2 5 1 
4 3 2 1 5 

4 3 1 2 5 
4 1 3 2 5 
1 4 3 2 5 

1 3 4 2 5 
1 3 2 4 5 

1 2 3 4 5 
```
