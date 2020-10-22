---
title: string-1
data: 2020-7-31
categories:
- 算法题
- String
---

## Leetcode125验证回文串

```cpp
bool check(char c){
        if( c >= 'a' && c <= 'z' || c >= '0' && c <='9' || c >='A' && c <= 'Z')
            return true;
        return false;
    }
    bool isPalindrome(string s) {
        int i, j;
        for(i = 0, j = s.size(); i < j ; i++, j--){
            while(i < j && !check(s[i]))i++;
            while(i < j && !check(s[j]))j--;
            if(i < j && tolower(s[i]) != tolower(s[j]))
                return false;
        }
        cout<<"i="<<i<<" j="<<j;
        return true;
    }

```



## 找出最长回文串

1. 思路：枚举每个字符为回文串的中点位置，向两边散开
2. 代码

   ```c++
    string longestPalindrome(string s) {
        if(s.size() == 0 || s.size() == 1)return s;
        string res = "";
        for(int i = 0;i < s.size(); i++){
            int j = i+1, k = i;
            while(k >= 0&& j<s.size() && s[k] == s[j]){k--,j++;}
            if(j-k-1 > res.size())res = s.substr(k+1, j-k-1);
            j = i+1 ,k = i-1;
            while(k >= 0 && j<s.size() && s[k] == s[j]){k--,j++;}
            if(j-k-1 > res.size())res = s.substr(k+1, j-k+1);
        }
        return res;
    }

   ```

### 解决办法2

## Leetcode131分割回文串

```cpp
vector<vector<bool>> dp;//i到j是否是回文串， i <= j
    vector<vector<string>> res;
    vector<string> path;
    vector<vector<string>> partition(string s) {
        int n = s.size();
        dp = vector<vector<bool>>(n, vector<bool>(n));
        for(int j = 0; j < n; j++){//枚举结束位置，正三角(矩形的下半部分)
            for(int i = 0; i <= j; i++){
                if(i == j)dp[i][j] = true;
                else if(s[i] == s[j]){
                    //只有两个字符        //通常情况
                    if(i + 1 > j - 1 || dp[i + 1][j - 1])dp[i][j] = true;
                }
            }
        }
        dfs(s, 0);
        return res;
    }
    void dfs(string& s, int index){
        if(index == s.size())res.push_back(path);
        else{
            for(int i = index; i < s.size(); i++){
                if(dp[index][i]){ // 枚举每个结束位置
                    path.push_back(s.substr(index, i - index + 1));
                    dfs(s, i + 1);
                    path.pop_back();
                }
            }
        }
    }
```

1. 字符串哈希加上二分查找 思路：

## 前缀后缀中间缀之类

### 最长公共前缀

取第一个字符串为标杆，后面依次做比较,如果完全相同，则返回第一个字符

```cpp
 string longestCommonPrefix(vector<string>strs) {
       string res ="" ;
       if ( strs.size() == 0 )
            return res;
        for(int i = 0; i < strs[0].size(); i++){
            char c = strs[0][i];
            for(int j=1 ; j < strs.size(); j++){
                if(i==strs[j].size() ||c != strs[j][i])
                    return strs[0].substr(0,i);
            }
        }
        return strs[0];
    }


```
