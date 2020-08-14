---
title: sudoku
date: 2020-08-14 10:46:27
tags:
- 数学问题
- 数独
categories:
- 算法题
- Trick
---

## 有效的数独LeetCode36

简单的模拟，先行检查，再列检查，在九宫格检查

```cpp
void memrset(vector<bool>& st){
        for(auto c : st)
            c = false ;
    }
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<bool> st(9,false);
        //枚举行
        for(int i = 0; i < 9 ;i++){
            memrset(st);
            for(int j = 0 ;j < 9;j++){
                if(board[i][j] == '.')continue;
                if(st[board[i][j] - '1'])return false;
                st[board[i][j] - '1'] =  true;
            }
        }
        //枚举列
        for(int i = 0; i < 9 ;i++){
            memrset(st);
            for(int j = 0 ;j < 9;j++){
                if(board[j][i] == '.')continue;
                if(st[board[j][i] - '1'])return false;
                st[board[j][i] - '1'] =  true;
            }
        }
        //判断九宫格
        for(int i = 0 ;i< 9; i +=3 ){
            for(int j = 0; j < 9; j +=3){//每个九宫格的起点
                memrset(st);
                for(int x = 0; x < 3; x++)
                    for(int y = 0; y < 3; y++){
                        if(board[i + x][j + y] == '.')continue;
                        if(st[board[i + x][j + y] - '1'])return false;
                        st[board[i + x][j + y] - '1'] = true;
                    }
            }
        }
        return true;
    }
```

## 解数独LeetCode37

看注释吧，写的明明白白

```cpp
bool row[9][9]  = {false};//判断每行上的某个数字是否用过
    bool col[9][9]  = {false};//判断每列上某个数字是否用过
    bool cell[3][3][9] = {false};//判断每个九宫格上的数字是否用过
    void solveSudoku(vector<vector<char>>& board) {
        for(int  i = 0; i < 9; i++){
            for(int j = 0; j < 9; j++){
                char c = board[i][j];
                if(c != '.'){//当前位置已经被填充过了
                    int t = c - '1';
                    row[i][t] = col[j][t] = cell[i/3][j/3][t] = true;
                }
            }
        }
        dfs(board, 0, 0);
    }
    bool dfs(vector<vector<char>>&board, int x,int y){//x y表示当前要填充的位置
        if(y == 9)x++, y = 0;//当前行被填完了，填下一行
        if(x == 9)return true; //所有行都被填完了，找到一个解
        if(board[x][y] != '.')return dfs(board, x , y+1);
        for(int i = 0; i < 9; i++){// 枚举1~9这些数字
            if(!row[x][i] && !col[y][i] && !cell[x/3][y/3][i]){//当前数字是有效的(没被用过)
                char c = '1' + i;
                board[x][y] = c;
                row[x][i] = col[y][i] = cell[x/3][y/3][i] = true;
                if(dfs(board, x ,y+1))return true;//找到之后就返回了，填充的结果都能被保存
                row[x][i] = col[y][i] = cell[x/3][y/3][i] = false;
                board[x][y] = '.';

            }
        }
        return false;
    }
```