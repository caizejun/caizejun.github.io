---
title: linkedlist-swap
date: 2020-08-25 17:30:40
tags:
categories:
- 算法题
- linkedList
---

这类题目大同小异，都是枚举要反转的这一段的节点前一个节点

## Leetcode24两两交换链表中的节点

```go
func swapPairs(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }
    dummy := &ListNode{-1,head}
    cur := dummy
    for cur !=nil && cur.Next != nil && cur.Next.Next != nil {
        a := cur.Next
        b := a.Next
        c := b.Next
        a.Next = c
        cur.Next = b
        b.Next = a
        cur = a
    }
    return dummy.Next
}
```

## Leetcode25K个一组反转链表

```cpp
    if(!head)return nullptr;
    ListNode *dummy = new ListNode(-1);
    for(auto p = dummy; ;){
        auto q = p;// p就是前一个节点
        for(int i = 0; q && i < k; i++)q = q ->next;
        if(!q)break;
        auto nex = q ->next; //下一段的第一个节点
        auto a = q -> next;
        auto tmp = a;
        auto b = a -> next;
        while(b != nex){
            auto c = b -> next;
            b -> next = a;
            a = b; 
            b = c;
        }
        p -> next = a;
        tmp -> next = nex;
        p = tmp;
    }
    return dummy ->next;
```