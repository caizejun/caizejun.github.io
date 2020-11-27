---
title: raftpaper
date: 2020-08-17 17:05:23
tags:
categories:
- 公开课
- mit_6.824
---

## 概要

Raft 是一种基于日志复制的一致性算法，相较于Paxos，更容易理解，更易于工程实现。</br>
**Raft 相较于其他一致性算法自身较强的特点：</br>**

1. Strong leader: Raft 使用了一种比其他一致性算法更强的Leader。比如，日志记录只能从leader复制到其他机器，这样可以简化日志复制和降低理解难度。

2. Leader election: Raft使用了随机定时器来选择leader。通过在心跳上增加一些小的改动，可以简单快速的解决冲突。

3. Membership changes: Raft使用了一种联合一致性的方法，使得集群中的机器发生变更的时候。整个集权也可以正常的工作。

**为了使得Raft算法更易于理解，采用了两种通用的技术：**

1. 划分子问题：将Raft拆分为四个独立的问题，一致性问题拆分：

* 选主
* 日志复制
* 安全保障
* 成员变更

2. 减少需要考虑的状态来简化状态空间:</br>

例如日志中不允许出现空操作，空洞可能导致集群的不一致

## Raft基础

任意时刻，一台服务器会处在三种状态下的一种：leader、follower，candidate。下面的表格列出了这些状态的表示参数和这些状态的迁移。

| State(每台机器上的状态参数)|
| :---  |
|**Presistent State on all servers(在RPC响应之前,需要更新稳定存储介质):**</br>**currentTerm**: latest term server has seen (initialized to 0 on first boot, increases monotonically)</br>**votedFor:** candidateId that received vote in current term (or null if none)</br>**log[]:** each entry contains command for state machine, and term when entry was received by leader (first index is 1)|
|**Volatile state on all servers(所有机器的可变状态)**:</br> **commitIndex:** index of highest log entry known to be committed (initialized to 0, increases monotonically)</br> **lastApplied:** index of highest log entry applied to state machine (initialized to 0, increases monotonically)|
|**Volatile state on leaders(leader的状态,在选举之后重新初始化):**</br> **nextIndex[]：** for each server, index of the next log entry to send to that server (initialized to leader last log index + 1)</br> **matchIndex[]:** for each server, index of highest log entry known to be replicated on server (initialized to 0, increases monotonically)
|
