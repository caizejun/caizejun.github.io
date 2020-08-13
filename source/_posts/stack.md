---
title: stack
date: 2020-08-12 16:39:05
tags:
categories:
- 算法题
- dataStructure
---

## 括号匹配问题LeetCode32

>> 输入: "(()" &emsp;")()())"
输出: &ensp;2   &emsp;&emsp;4

括号类问题以后都用栈来处理吧，形成一个统一的规范。</br>
在一个有效的括号的序列和可能有效的括号子序列里一定满足以下条件:</br>

* 左括号的数量等于右括号的数量(立即有效)
* 左括号的数量大于等于右括号的数量(可能在将来不久有效)

那么推论得出在左括号数量小于等于右边括号的数量这一情况下，一定不会存在，将来也不会可能出现合法的括号序列，所以重新选择当前子串的起始位置

```cpp
 int longestValidParentheses(string s) {
        int res = 0;
        stack<int> stk ;
        for(int i = 0, start = -1; i < s.size(); i++){
            if(s[i] == '(')stk.push(i);
            else{
                if(stk.size()){
                    stk.pop();
                    if(stk.size()) {//(((())
                        res = max(res, i - stk.top());
                    }else{//()() 
                        res = max(res, i - start);
                    }
                }
                else{
                    start = i;
                }
            }

        }
        return res;
    }
```
