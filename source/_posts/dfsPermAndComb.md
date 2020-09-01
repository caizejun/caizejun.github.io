---
title: dfsPermAndComb
date: 2020-08-28 10:54:30
tags:
- 算法题
- Search
---

经典老番，关键是利用相对位置的思想来进行去重，在递归树的每一层，注意当前这棵树是从哪个角标开始的

## 生成排列

生成排列一般需要一个辅助数组</br>

1. 如果是枚举每个数字放到哪个位置上，辅助数组就用来标定当前数字是否被用过

2. 如果是枚举每个位置上放哪个数，辅助数组就用来标定当前位置是否被用过 

个人的常用思想是枚举数字，放到合法的位置，pos用来指示当前位置是否合法（是否被用过）

### 无重复元素

```cpp
    void dfs(int index){
        if(index == n){
            res.push_back(path);
            return;
        }
        for(int i = 0; i < n; i++){
            if(!pos[i]){
                pos[i] = true ;
                path[i] = nums[index];
                dfs(index + 1)//处理下一个数字
                path[i] = false;
            } 
        }
    }

```

### 有重复元素

去重的思想就是先排序，之后保持相同的元素的相对位置不变

```cpp
    sort(nums.begin(), nums.end());
    void dfs(int begin, int index){ //index是下一个处理的数字，begin是该数字可以放的位置
        if(index == n){
            res.push_back(path);
            return;
        }
        for(int i = begin, i < n; i++){
            if(!pos[i]){
                pos[i] = true;
                path[i] = nums[index];
                dfs(index + 1  < n && nums[index + 1] ==nums[index] ? i + 1: 0, index + 1)
            }
        }

    }
```

## 生成组合

基本规则是只能拿在自己后面的数字进行组合，

### 无重复

```cpp
    vector<int> path
    void dfs(int index){
        res.push_back(path);
        return 
    }
    for(int i = index; i < n ; i++){
        path.push_back(nums[i]);
        dfs(i + 1);
        path.pop_back();
    }

```

### 有重复

```cpp  
    void dfs(int index){
        res.push_back(path);
        for(int i = index; i < n; i++) {
            if(i > index && nums[i] == nums[i-1])continue;
            path.push_back(nums[i]);
            dfs(i + 1, nums);
            path.pop_back();
        }
    }



```