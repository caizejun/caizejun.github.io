---
title: hashTable
date: 2020-08-05 11:12:25
tags:
categories: 
- 算法题
- dataStructure
---


## 哈希表（查询表）

### 数字转罗马数字

```golang

var nums []int = []int{ 1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1}
var chars []string=[]string{"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I",}
func intToRoman(num int) string {
    res := ""
    for i, n := range nums{
        for num >= n {
            num -= n;
            res += chars[i]
        }
    }
    return res
}

```
