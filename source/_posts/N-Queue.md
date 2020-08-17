---
title: N-Queue
date: 2020-08-17 10:37:44
tags:
- 智力题
categories:
- 算法题
- Search
---

## 经典N皇后问题

```cpp
vector<int> vis;//第i行皇后放在了哪一列
    int n;
    vector<vector<string>> res;
    vector<vector<string>> solveNQueens(int n) {
        vis = vector<int>(n,0);
        this->n = n;
        dfs(0);
        return res;
    }
    void dfs(int k){//放置第k个皇后
        if(k == n) 
        {
            vector<string> path(n,"");
            for(int i = 0; i < n; i++){
                for(int j = 0; j < n; j++)
                    path[i]+='.';
            }
            for(int i = 0; i < n; i++)
                path[i][vis[i]] = 'Q';
            res.push_back(path);
            return;
        }
        for(int i = 0 ;i < n; i++){//试探当前第k行的皇后能放在哪一列
            int j;
            for( j = 0; j < k; j++){
                //1. 第j行的皇后不能在第i列 2 之前的皇后不能与当前皇后在对角线，列差 == 行差
                if(vis[j] == i || abs(i - vis[j]) == abs(k - j))
                    break;
            }
            if(j == k){
                vis[k] = i;
                dfs(k + 1);
            }
        }

    }
```
