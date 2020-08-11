---
title: linkedList-1
date: 2020-08-10 09:57:33
tags:
categories:
- 算法题
- linkedList
---

## 链表两两交换节点

### 思路

枚举两个节点的前面一个节点

### 代码

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

## 删除链表倒数第k个节点

### 思路

经典找到倒数第k个节点的前驱，使用快慢指针。  然后删除第k个节点

### 代码

```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummy := &ListNode{-1,head}
    fast, slow := dummy, dummy
    for n>0 {
        fast = fast.Next
        n--
    }
    for fast.Next != nil {
        fast = fast.Next
        slow = slow.Next
    }
    slow.Next = slow.Next.Next
    return dummy.Next
}
```

