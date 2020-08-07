---
title: dfs-1
date: 2020-08-07 11:09:17
tags:
categories:
- 算法题
- Search
---


## 电话号码组合Leetcode17

思路：顺着每个数字搜，枚举每个数字对应的字母，将每个数字每趟对应的字母进行组合
代码：

```golang
var table []string =[]string{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"}
var res []string
var n int = 0 ;
var out string ;
func letterCombinations(digits string) []string {
    res = []string{}
    if len(digits) == 0 {
        return res
    }
    n = len(digits)
    dfs(0, out, digits)
    return res
}

func dfs(index int, out, digits string){
    if(index == n){
        res = append(res, out)
        return
    }
    for i := 0; i < len(table[(int)(digits[index] - '0')]); i++ {
        c :=(string)(table[(int)(digits[index] - '0')][i])
        out += c
        dfs(index+1, out, digits,)
        out = out[0 : len(out)-1]
    }
}
```
