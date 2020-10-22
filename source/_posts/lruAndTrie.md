---
title: lruAndTrie
date: 2020-09-14 17:28:13
tags:
categories:
- 算法题
- advancedStruct
---

## Leetcode146 LRU缓存机制

自己实现的链表加上stl里面的unordered_map

```cpp
class LRUCache {
public:
    struct Node{
        int key;
        int value;
        Node *nex, *pre;
        Node(int _key, int _val): key(_key), value(_val), pre(NULL), nex(NULL) {}
    }*head, *tail;
    unordered_map<int, Node*> hash;
    int cap, len;
    LRUCache(int capacity) {
        this -> cap = capacity;
        this -> len = 0;
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        head -> nex = tail;
        tail -> pre = head;
    }
    void remove(Node *p){
        p -> pre -> nex = p -> nex;
        p -> nex -> pre = p -> pre;
    }
    void push_front(Node *p) {
        p -> nex = head -> nex;
        head -> nex -> pre = p;
        head -> nex = p;
        p -> pre = head;
    }
    int get(int key) {
        if(hash.count(key) == 0)return -1;
        auto ptr = hash[key];
        remove(ptr);
        push_front(ptr);
        return ptr -> value;
    }
    void put(int key, int value) {
        if(hash.count(key)){
            Node *ptr = hash[key];
            ptr -> value = value;
            remove(ptr);
            push_front(ptr);
        }else{
            if(hash.size() == cap){
                Node *end = tail -> pre;
                remove(end);
                hash.erase(end -> key);
                delete end;
            }
            Node *new_Node = new Node(key, value);
            hash[key] = new_Node;
            push_front(new_Node);
        }

    }
};

```
