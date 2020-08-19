---
title: brainteaser-2
date: 2020-08-18 11:22:19
tags:
- 模拟
categories:
- 算法题
- Trick
---
## Leetcode43字符串相乘

简单的模拟乘法

```cpp
string multiply(string num1, string num2) {
        if(num1 == "0" || num2 == "0")return "0";
        int size_1 = num1.size(), size_2 = num2.size();
        vector<int> res_vec(size_1+size_2,0);
        for(int i = size_1-1; i >= 0; i--){
            int val1 = num1[i] - '0';
            for(int j = size_2 - 1; j >= 0; j--){
                int val2 = num2[j] - '0';
                int val = val1 * val2 + res_vec[i + j + 1];//这里的处理是最重要的，要加上上一次计算得到的进位
                res_vec[i+j+1] = val % 10;
                res_vec[i+j] += val / 10;
            }
        }
        string res = "";
        int k = 0 ;
        while(res_vec[k] == 0) k++;
        for(int i = k ; i< res_vec.size(); i++)
            res += to_string(res_vec[i]);
        return res;
    }

```