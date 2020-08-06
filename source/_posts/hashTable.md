---
title: hashTable
date: 2020-08-05 11:12:25
tags:
categories: 
- 算法题
- dataStructure
---


## 哈希表（查询表）

### 罗马数字转罗马数字

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

### 罗马数字转阿拉伯数字

应该也算双指针吧，pre是当前处理的字符，cur是用来决定对pre的处理

```golang

var table map[string]int = map[string]int{ "V":5, "I":1, "X":10 ,"L":50, "C":100, "D":500, "M":1000,}
func romanToInt(s string) int {
    pre, res := 0, 0
    if len(s) == 0 {
        return res
    }
    pre = table[s[0:1]]
    for _, c := range s[1:] {
        cur := table[string(c)]
        if cur > pre {
            res -= pre
        }else{
            res += pre
        }
        pre = cur
    }
    return res + pre
}

```
