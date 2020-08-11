---
title: heap
date: 2020-08-10 14:39:09
tags:
categories:
- 算法题
- dataStructure
---

## 最小堆LeetCode23

题目 合并几个有序链表

```cpp
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.size() == 0) return NULL;
        priority_queue<pair<int,ListNode*>> heap;//默认大顶堆
        ListNode *dummy =new ListNode(-1);
        ListNode *cur = dummy;
        for (auto c :lists)if(c) heap.push({-c->val,c});//将数值取相反数，就成了小顶堆
        while(heap.size()){
            auto  p = heap.top();
            heap.pop();
            cur = cur ->next = p.second;
            auto tmp = p.second->next;
            if(tmp) heap.push({-tmp->val, tmp});
        }
        cur -> next = NULL;
        return dummy -> next;
    }

```
