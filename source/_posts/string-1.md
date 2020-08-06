---
title: string-1
data: 2020-7-31
categories:
- 算法题
- String
---


## 回文串

### 解决办法1

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
