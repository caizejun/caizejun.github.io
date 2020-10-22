---
title: dp_string
date: 2020-09-23 15:04:25
tags:
categories:
- 算法题
- DynamicProgramming
---

## Leetcode115 不同的子序列

(动态规划) O(nm)
可以换一种考虑问题的方式：用 S 中的字符，按顺序匹配 T 中的字符，问有多少种方式可以匹配完 TT 中的所有字符。

可以用动态规划来做：
f[i][j] 表示用 S 的前 i 个字符，能匹配完 T 的前 j 个字符的方案数。
初始化：因为 SS 可以从任意一个字符开始匹配，所以 f[i][0]=1,∀i∈[0,len(S)]
状态转移：
如果 S[i−1]≠T[j−1]，则 S[i−1] 不能匹配 T[j−1]，所以 f[i][j]=f[i−1][j]</br>
如果 S[i−1]=T[j−1]，则 S[i−1] 既可以匹配 T[j−1]，也可以不匹配 T[j−1]，所以 f[i][j]=f[i−1][j]+f[i−1][j−1]</br>
时间复杂度分析：假设 S 的长度是 n，T 的长度是 m，则共有 nm个状态，状态转移的复杂度是 O(1)，所以总时间复杂度是 O(nm)。

```cpp
 //用S里面的字符去匹配T里面的字符
    int numDistinct(string s, string t) {
       int s_size = s.size(), t_size = t.size();
       s = ' '+ s, t = ' ' + t;
       vector<vector<long long>> dp(s_size + 1, vector<long long>(t_size + 1));
       for(int i = 0; i <= s_size; i++)dp[i][0] = 1;
       for(int i = 1; i <= s_size; i++){
           for(int j = 1; j <= t_size; j++){
               dp[i][j] = dp[i - 1][j];
               if(s[i] == t[j]) dp[i][j] += dp[i - 1][j - 1];
           }
       }
       return dp[s_size][t_size];
    }
```

## 正则表达式匹配

思路: 应该从后向前匹配,关键是模式串和文本串最后的空格一定为true;

1. 状态表示：dp[i][j] 模式串str从下标i到串结束位置 和匹配串pat从下标i到串结束位置的字串
2. 属性 ：以上两个串是否能匹配
3. 状态转移：如果str[i]==pat[j],则dp[i][j]=dp[i+1][j+1],如果pat[j+1]='*',那么dp[i][j]=dp[i][j+2]
4. 代码：

   ```c++
   dp[i][j]=true; //两个串结尾的空串一定能匹配
   for(int i = n;i>=0;i--)
    for(int j=m-1; j>=0;j--){
        bool firstMatch = i<n && (str[i]==pat[j]||pat[j]==*);
        if(j+1 < m && pat[j+1] == '*'){
            dp[i][j] = dp[i][j+2]||(firstMatch&&dp[i+1][j]//匹配0个或者多个 例如c a*c 或者aaac a*c
        }else{
            dp[i][j] = firtsMatch&&dp[i+1][j+1];//通常情况下，要保证之前已经匹配完成
        }
    }
    return dp[0][0]
   ```

## Leetcode44通配符匹配

```cpp
bool isMatch(string s, string p) {
        int n = s.size(), m = p.size();
        vector<vector<bool>> dp(n+1, vector<bool>(m+1,false));
        dp[n][m] = true;
        for(int i = n; i >=0; i--){
            for(int j = m-1; j>=0; j--){//边界是匹配空字符
                bool firtsmathch = i < n && (s[i] == p[j] || p[j] == '?');//
                if(p[j] != '*'){//不能匹配空字符边界，用firstmatch屏蔽掉了
                    dp[i][j] =  firtsmathch && dp[i+1][j+1];
                }else{
                               //*不需要起作用   //需要起作用
                    dp[i][j] = dp[i][j+1] || ((i + 1 <= n) && dp[i+1][j]);
                                            //这里也是边界不好搞

                }
            }
        }
        return dp[0][0];
    }
```

## 编辑距离

1. 状态表示：dp[i][j] 将串A的前i个字符变成串B的前j个字符需要多少次处理
2. 状态转移：一共有三种处理方法,在三种处理方法里去最小值，假设正在处理A[i]和B[j]：
   1. 删除 dp[i][j]=dp[i-1,j]+1
   2. 替换 dp[i][j]=dp[i-1,j-1]+(A[i]!=B[j])
   3. 插入 dp[i,j]=dp[i,j-1]+1
3. 代码：

   ```c++
    class Solution {
        public:
        int minDistance(string word1, string word2) {
            int n = word1.size(), m = word2.size();
            if (!n || !m) return n + m;
            vector<vector<int>>f = vector<vector<int>>(n + 1, vector<int>(m + 1));
            f[0][0] = 0;
            for (int i = 1; i <= n; i ++ ) f[i][0] = i;
            for (int j = 1; j <= m; j ++ ) f[0][j] = j;
            for (int i = 1; i <= n; i ++ )
                for (int j = 1; j <= m; j ++ )
                {
                    f[i][j] = f[i - 1][j - 1] + (word1[i - 1] != word2[j - 1]);
                    f[i][j] = min(f[i][j], f[i - 1][j] + 1);
                    f[i][j] = min(f[i][j], f[i][j - 1] + 1);
                }
            return f[n][m];
        }
    };

    ```
