---
title: dfs-1
date: 2020-08-07 11:09:17
tags: 
- 算法
- 搜索
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

## 括号生成Leetcode22

递归树大概长这样</br>

```c
                        (
            ((                      ()
    (((         (()             ()(     
((()            (())         ()()
```

代码：

```golang
class Solution {
public:
    vector<string> res;
    int  k;
    vector<string> generateParenthesis(int n) {
        this -> k = n; 
        dfs(0,0,"");
        return res;

    }
    void dfs(int l, int r ,string out){
        if(l==k && r==k ){
            res.push_back(out);
            return;
        }
        if(l < k)dfs(l+1,r,out+'(');
        if(l > r)dfs(l,r+1,out+')');
    }
};

```
