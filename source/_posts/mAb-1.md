---
title: mAb-1
date: 2020-08-04 11:26:34
tags:
categories:
- 算法题
- MathAndBitManipulation
---

## 实现除法Leetcode29

不能出现乘除，模运算，实现除法。
思路： 得到除数的倍数，被除数从最大的倍数开始减，就可以了。

```cpp
typedef long long LL;

class Solution {
public:
    int divide(int dividend, int divisor) {
        bool neg = false ;
        vector<LL> exp ;
        if((dividend > 0 && divisor < 0) ||(dividend < 0 && divisor > 0)){
            neg = true;
        }
        LL a = abs((LL)dividend), b = abs((LL)divisor), res = 0;
        for(LL i = b ; i <= a; i += i )
            exp.push_back(i);
        for(int i = exp.size()-1; i >= 0; i--){
            if(a >= exp[i]){
                a -= exp[i];
                res += 1LL << i;
            }
        }
        if(neg) res = -res;
        if(res > INT_MAX || res< INT_MIN)return INT_MAX;
        return (int)res;
    }
};


```cpp






## 一些特殊的性质

高级语言里，负数mod正数，结果依旧是负数 -1%10=-1, -1234%10=-4；
但是数学里面具体算法就不知道了

## 回文数字

```cpp
 bool isPalindrome(int x) {
        if(x<0 || x&&x%10==0)return false;//x%10==0一定不是回文数
        int s = 0;
        while(s <= x){
            s = s * 10 + x % 10;
            if(s == x|| s== x/10)return true;//可能x是123321这样的数，判断s==x/10可以加快速度
            x /= 10;  
        }
        return false;
    }


```
