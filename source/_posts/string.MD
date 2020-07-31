---
title: 字符串经典问题-1
data: 2020-7-31
catagories:
- 算法题
- 字符串

---


## 1. 回文串

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
   