---
title: dp-1
data: 2020-7-30
categories:
- 算法题
- DynamicProgramming
---

## Leetcode53最大和子数组

这个最巧妙的是对边界的处理吧，还有怎么用滚动数组

```cpp
int maxSubArray(vector<int>& nums) {
       int res = INT_MIN;
       for(int i = 0, last = 0; i < nums.size(); i++){
           last = nums[i] + max(last, 0);//当前为负数肯定i就不会加进来了
           res = max(res, last);
       }
       return res;
    }


```

## Leetcode152最大乘积子序列

子数组里的最大乘积

```cpp
int maxProduct(vector<int>& nums) {
        int res = nums[0];
        for(int i = 1, f = nums[0], g = nums[0]; i < nums.size(); i++){
            int na = nums[i], fa = f * na, ga = g * na;
            f = max(na, max(fa, ga));
            g = min(na, min(fa, ga));
            res = max(f, res);
        }
        return res;
    }
```

## 最长上升子序列

思路：dp[i]    状态表示以下标i结尾的子串，属性为该字串的最长上升子序列

```cpp
for(int i=0; i<n ;i++){
    dp[i]=1;
    for(int j=0 ;j<i ;j++)
        if(nums[j]<nums[i])
            dp[i]=max(dp[i],dp[j]+1)
}

```

## 最长公共子序列

1. 状态表示：dp[i][j] 串A中以i下标结尾的字串和串B中以j下标为结尾的子串 两个字串公共子序列的最大长度
2. 转移方法：
   2.1 如果A[i]与B[j]不相等，

   `dp[i][j]=max(dp[i-1][j],dp[i][j-1])`
   2.2 如果A[i]与B[j]相等，

   `dp[i][j]=dp[i-1][j-1]+1)`
3. 代码

   ```c++
   dp[0][0]=0;
   for(int i=1 ;i<=n ; i++)
        for(int j=1 ;j<=m ;j++){
            if(A[i]==B[j])
                dp[i][j]=dp[i-1][j-1]+1;
            else{
                dp[i][j]=max(dp[i-1][j],dp[i][j-1])
            }
        }
   ```

