---
title: 简单模拟
date: 2020-08-03 09:50:16
tags: 算法 模拟
categories: 
- 算法题
- Trick
---

这类题目主要就是记得推导的数学公式吧，不记得的话现场推太离谱了
所以能多记一点题是一点

## 1. Z字形打印

* 题目意思: 字符串下标本来是 0 1 2 3 4 5 6 .....
   取n=3，做Z字变换，可以得到

  ```c
  0     4     8
  1  3  5  7  9
  2     6     10
  ```

* 思路：找数学规律了</br>
   2.1 首行和末尾行分别是以0 i-1作为首相，公差为n-1的等差数列</br>
   2.2 其他行是两个交错的等差数列，首相分别为i,2n-i,公差也相同</br>

* 代码：

   ```c++
   string convert(string s, int numRows) {
        if(!s.size()||numRows == 1)return s;
        string res = "";
        for(int i = 0 ;i< numRows;i++)
        {
            if(i == 0|| i == numRows-1){
                for(int j = i; j<s.size(); j += 2*(numRows-1) ){
                    res += s[j];
                }
            }
            else{
                for(int j = i, k = 2*(numRows-1) - i; j < s.size() || k<s.size() ; j += 2*(numRows-1),k += 2*(numRows-1)){
                    if(j < s.size())res += s[j];
                    if(k < s.size())res += s[k];
                }
            }
        }
        return res;
    }
   ```