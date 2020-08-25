---
title: goTips
date: 2020-08-25 16:19:35
tags:
categories:
- golang
- tips
---

## go的类型声明的设计

总所周知，go的类型声明与c, cpp, java等传统语言不太像，这些传统的语言都是变量类型放左边，变量名放右边</br>
像这样 `int x , int *p , int a[3]`</br>
以上是一些基本数据类型的声明，但是函数的类型声明就出来问题了，如下</br>

```cpp
    int cal(int)
    int (*pf)(int)//这两个还好
    int (*fp)(int (*)(int, int), int)//这就有点不好看清了，忽略所有参数的参数名
    int (*(*fp)(int (*)(int, int), int))(int, int)//甚至返回值也是一个指针，直接自闭

```

go的类型声明主要就很好的规避了上面这样的情况</br>
`f func(func(int,int) int, int) func(int, int) int` 依旧可以读的清楚，参数列表，返回值

## go的基本数据类型和复合数据类型

