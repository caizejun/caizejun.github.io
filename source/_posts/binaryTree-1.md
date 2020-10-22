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

### Leetcode98 验证某棵树是否为二叉搜索树

```cpp
bool isValidBST(TreeNode* root) {
        return dfs(root, INT_MIN, INT_MAX);
    }
    bool dfs(TreeNode* root, LL min, LL max){
        if(!root)return true;
        if(root -> val < min || root -> val > max)return false;
        return dfs(root -> left, min, root -> val -1ll) && dfs(root -> right, root -> val + 1ll, max);
    }
```

### 判断两棵树是否相同

```cpp
bool check(TreeNode *p, TreeNode *q){
    (!q || !p)return !p && !q;
    return p -> val == q -> val && check(p -> left, q -> left) && check(p -> right, q -> right);
}
```

## 与数学有关的树

### Leetcode94, 95自然序列生成二叉搜索数(卡特兰数)

两个题目一个求方案数，一个求具体的组合方法</br>
求方案数可以用动态规划，dp[i]表示以i为根节点的二叉搜索树有多少种</br>
那么dp[i] = dp[j] * dp[k], j(0 ~ i - 1)为左子树的节点个数， k( i - 1 ~ 0)为右子树的节点个数

```cpp
int numTrees(int n) {
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        for(int i = 1; i <= n; i++){ //枚举哪个数作为根节点
            for(int j = 1; j <= i; j++){     //枚举左右子树节点的个数
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
```

求组合方法和上面思路差不多，枚举根节点，拿到左右子树的集合

```cpp
 vector<TreeNode*> generateTrees(int n) {
        if(!n) return {};
        return dfs(1, n);
    }
    vector<TreeNode*> dfs(int l, int r){
        if(l > r)return {nullptr};
        vector<TreeNode*> res;
        for(int i = l ; i <= r; i++){
            auto left = dfs(l, i - 1), right = dfs(i + 1, r);
            for(auto l : left)
                for(auto r : right){
                    auto root = new TreeNode(i);
                    root -> left = l, root -> right = r;
                    res.push_back(root);
                }
        }
        return res;
    }
```
