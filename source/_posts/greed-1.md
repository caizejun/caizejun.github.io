---
title: greed-1
date: 2020-08-19 10:49:25
tags:
categories:
- 算法题
- greed
---
***贪心怎么说呢，没有见过真的难想，比dp问题还难想，所以多积累吧***

## Leetcode55 跳跃游戏

在每个坐标上，尽可能多的往前跳，并且保证当前访问的坐标是可以到达的，最后判断能到的坐标是不是大于最后一个坐标

```cpp
bool canJump(vector<int>& nums) {
        int maxDis = 0 ;
        for(int i = 0; i < nums.size() && i <= maxDis; i++){
            if(i + nums[i]> maxDis) maxDis = i + nums[i];
        }
        return maxDis >= nums.size() - 1;
    }
```
