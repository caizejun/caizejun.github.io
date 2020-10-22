---
title: 简单模拟
date: 2020-08-03 09:50:16
tags: 
- 算法 
- 模拟
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

## Leetcode41 缺失的第一个正整数

给一个未排序数组，找到数组中缺失的最小正整数</br>

解法：将数组中的所有正整数尽可能移动到对应的角标下(角标和数字相同)，遍历一次数组
&emsp;移动完成后，再从前往后遍历，看哪个角标和该位置上的数不相同，则是所求的数

```cpp
int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        if(n == 0) return 1;
        for(auto& c : nums)
            if(c != INT_MIN)c--;//做这个操作是为了下面的比较方便
        for(int i = 0 ;i < n; i++){
            while(nums[i] < n && nums[i] >= 0 && nums[i] != i nums[nums[i]] != nums[i]){
                swap(nums[i], nums[nums[i]]);
            }
            pri(nums);
        }
        for(int i = 0; i < n ; i++){
            if(nums[i] != i)
                return i+1;
        }
        return n+1;
}
```

## Leetcode89 格雷编码

格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差</br>
设第K+1个编码序列为S(k+1), 则S(k+1)可由Sk 推出来</br>
比如:</br>
k = 2 编码序列为00 01 11 10</br>
则 k = 3, 可以先将 k = 2 的序列先做对称，得到 00 01 11 10 | 10 11 01 00</br>
再在前半部分后面补0, 后半部分补1,得到如下</br>
>000 010 110 100 | 101 111 011 001

```cpp
vector<int> grayCode(int n) {
        vector<int> res(1, 0);
        while(n--){
            for(int i = res.size() - 1; i >= 0; i--){
                res[i] *= 2; //补0只要乘以2
                res.push_back(res[i] + 1);
            }
        }
        return res;
    }


```