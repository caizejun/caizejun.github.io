---
title: towPointer-1
date: 2020-08-06 14:51:31
tags:
categories:
- 算法题
- twoPointer
---

## 三数之和 Leetcode15

枚举每个数字，找其他两个数

```cpp

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        if(nums.size()<3)return {};
        vector<vector<int>>res;
        sort(nums.begin(),nums.end());
        for(int i=0;i<nums.size()-2;i++){
            if(nums[i]>0)break;                      //去重
            if(i==0||(i>0&&nums[i-1]!=nums[i])){     //去重
                int c=-nums[i];
                int j=i+1,k=nums.size()-1;
                while(j<k){
                    if(nums[j]+nums[k]==c){
                        res.push_back({nums[i],nums[j],nums[k]});
                        while(j<k&&nums[j+1]==nums[j])j++;//关键去重 类似情况 -1 0 0 1 1
                        while(j<k&&nums[k]==nums[k-1])k--;
                        j++,k--;
                    }
                    else if(nums[j]+nums[k]>c)k--;
                    else j++;

                }
            }
        }
        return res;
    }
};







```

