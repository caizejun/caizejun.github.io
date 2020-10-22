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

## Leetcode85最大矩形

经典单调栈</br>

首先算是一个模板，给一个数组，每个元素代表在该坐标上的高度，求一个最大的矩阵

```cpp
int largestRectangleArea(vector<int>& heights) {
        if(!heights.size())return 0;
        int n = heights.size();
        vector<int> left(n), right(n);
        stack<int> stk;
        for(int i = 0; i < n ; i++){
            while(stk.size() && heights[i] <= heights[stk.top()])stk.pop();
            if(stk.size())left[i] = stk.top();
            else left[i] = -1;
            stk.push(i);
        }
        while(stk.size())stk.pop();
        for(int i = n - 1; i >=0; i--){
            while(stk.size() && heights[i] <= heights[stk.top()])stk.pop();
            if(stk.size())right[i] = stk.top();
            else right[i] = n;
            stk.push(i);
        }
        int res = INT_MIN;
        for(int i = 0; i < n; i++){
            res = max(res, heights[i] * (right[i] - left[i] - 1));
        }
        return res;
    }
```

然后是在01矩阵里找出最大的1矩阵(全部由1构成)</br>

思路：将这个矩阵拆分开来，每一行都当作上一个问题的参数，求出来最大值就行了。</br>
问题的关键是求出来每一行的高度

```cpp
int maximalRectangle(vector<vector<char>>& matrix) {
        if(!matrix.size() || !matrix[0].size())return 0;
        int n = matrix.size(), m = matrix[0].size();
        //以i,j为底的矩形的高度
        vector<vector<int>> h(n, vector<int>(m));
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(matrix[i][j] == '1'){
                    if(i)h[i][j] = 1 + h[i-1][j];
                    else h[i][j] = 1;
                }

            }
        }
        int res = 0;
        for(int i = 0; i < n; i++){
            res = max(res, largestRectangleArea(h[i]));
        }
        return res;
    }
```
