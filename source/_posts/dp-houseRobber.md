---
title: dp-houseRobber
date: 2020-10-22 16:55:46
tags:
- 算法题
- DynamicProgramming
---

## Leetcode198只能从前往后（房子做横向排列）

```cpp
    //d表示偷当前房子，p表示不取当前房子
    int rob(vector<int>& nums){
        int n = num.size();
        vector<int> d(n + 1), p(n + 1);
        for(int i = 1; i <= n; i++){
            d[i] = p[i - 1] + nums[i - 1];
            p[i] = max(d[i - 1], p[i - 1]);
        }
        return max(d[n], p[n]);
    }
```

## Leetcode213房子成环状排列


```cpp  
    int rob(vector<int>& nums){
        int n = nums.size();
        if(n == 0)return 0;
        if(n == 1)return nums[0];
        vector<int> d(n + 1), p(n + 1);
        //不取0号房子
        for(int i = 2; i <= n; i++){
            d[i] = p[i - 1] + nums[i - 1];
            p[i] = max(d[i - 1], p[i - 1]);
        }
        int res = max(d[n], p[n]);
        //取0号房子
        d[1] = nums[0], p[1]= INT_MIN;
        for(int i = 2; i <= n; i++){
            d[i] = p[i - 1] + nums[i - 1];
            p[i] = max(d[i - 1], p[i - 1]);
        }
        return max(res, p[n]);

    }



```
