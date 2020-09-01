---
title: linkedList-1
date: 2020-08-10 09:57:33
tags:
categories:
- 算法题
- linkedList
---

删除操作基本都是找要删除的节点的前一个节点，ptr -> next = ptr -> next -> next


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

## Leetcode203移除链表元素

这种相当于分类，把等于给定元素和不等于给定元素的做一个分类

```cpp
ListNode* removeElements(ListNode* head, int val) {
        ListNode *dummy = new ListNode(-1);
        for(auto p = dummy; p ;p = p -> next ){
            while(head && head -> val == val)head = head->next;
            p ->next = head ;
            if(head)head = head ->next;
        }
        return dummy -> next;
    }
```