---
title: dp-stockPrice
date: 2020-08-24 11:22:57
tags:
categories:
- 算法题
- DynamicProgramming
---

各种股票买入卖出,主要关注的是交易次数，不能同时参与多比交易（在再次购买前必须出售掉股票，可以理解为一天最多进行一次买入和卖出）

## LeetCode121只允许买入一次，卖出一次


思想就是贪心（动态规划简化），dp[i]为第i天卖出能获得的最大收益，找到[0~i]之间最小的价格。</br>
`dp[i] = max(dp[i-1],prices[i] - min(prices[j]))`</br>
又因为这个min是可以一直向后用的，所有可以用类似滚动数组

```cpp
 int maxProfit(vector<int>& prices) {
        int res = 0;
        for(int i = 0, minp = INT_MAX; i < prices.size() ; i++){
            res = max(res, prices[i] - minp);
            minp = min(minp, price[i]);
        }
        return res;
    }
```

## Leetcode122允许多次买入，多次卖出

允许当天卖出，再在当天买入

### 贪心算法

这个反倒是好理解一点，只要当天的售价比前一天高，那我就在今天卖出

```cpp
    res = 0;
    for(int i = 0; i + 1 < prices.size(); i++){
        res += max(0, prices[i+1] - prices[i])
    }
    return res;

```

### 动态规划

二维状态数组，dp[i][j]表示第i天在j情况下的最大利润，j = 0表示持有股票，j = 1 表示不持有股票，

状态转移:</br>

* 现在持有股票，前一天持有或者前一天未持有但在今天买入  dp[i][0] = max(dp[i-1][0], dp[i-1][1] - pirce[i])

* 现在不持有股票，说明前一日也不持有或者前一天持有但是在今天售出 dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - price[i])

```cpp
    dp[0][0] = -prices[1];
    dp[0][1] = 0
    for(int i = 1; i <prices.size(); i++){
        dp[i][0] = max(dp[i-1][0], dp[i-1][1] - prices[i]);
        dp[i][1] = max(dp[i-1][1], dp[i-1][0] + prices[i]);
    } 
    return dp[prices.size() - 1][1];

```

## 允许两次交易

### 前后缀分解

枚举第二次买入的时间，得出前面一段的最大值和后面一段的最大值

```cpp
        int n = prices.size();
        vector<int> slow(n);  //第一笔交易在第i天前完成（包含第i天）能获得的最大利润
        for(int i = 0, minp = prices[0]; i + 1< n; i++){
            slow[i+1] = max(slow[i], prices[i + 1] - minp);
            minp = min(minp, prices[i+1]);
        }
        int res = 0;
        //第二笔交易在第i天之后（包含第i天）开始能获得的最大利润,maxp表示最大的卖出价格
        for(int i = n - 1, maxp = 0; i >= 0; i--){
            res = max(res, maxp - prices[i] + slow[i]);
            maxp = max(maxp, prices[i]);
        }
        return res;
    }
```
