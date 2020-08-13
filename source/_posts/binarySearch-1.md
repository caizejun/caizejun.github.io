---
title: binarySearch-1
date: 2020-08-13 09:44:04
tags:
categories:
- 算法题
- bianarySearch
---

## 搜索旋转数组LeetCode33

```cpp
int search(vector<int>& nums, int target) {
        if(nums.size() == 0) return -1;
        //将数组分为两段，大于div和小于等于div
        //找到小于等于div的第一个数字
        int l = 0, r = nums.size()-1, div = nums[nums.size()-1];
        while(l < r){
            int mid = (l + r) >> 1;
            if(nums[mid] > div)
                l  = mid + 1;
            else
                r = mid;
        }
        if(target <= div )r=nums.size()-1;
        else
            r = r - 1, l=0;
        while(l < r){
            int mid = (l + r) >> 1;
            if(nums[mid] >= target )r= mid;
            else l = mid +1;
        }
        return nums[l] == target ? l:-1;
    }
