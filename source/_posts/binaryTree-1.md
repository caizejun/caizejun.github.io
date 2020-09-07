---
title: binaryTree-1
date: 2020-09-07 13:37:50
tags:
categories:
- 算法题
- binaryTree
---

## 与二叉树节点的值有关

### Leetcode113路径总和

找从根到叶节点总和是否等于某个值

```cpp
vector<int> path;
    vector<vector<int>> res;
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        dfs(root, sum);
        return res;
    }
    void dfs(TreeNode* root, int sum){
        if(!root)return;
        cout<<root -> val<<"..."<<sum<<endl;
        path.push_back(root -> val);
        sum -= root -> val;
        if(sum == 0 && root -> left == NULL && root -> right == NULL)
            res.push_back(path);
            //return 这里不能return 仔细想一下就知道了，return之后path就不对了
        if(root -> left)dfs(root -> left, sum);
        if(root -> right)dfs(root -> right, sum);
        path.pop_back();

    }
```
