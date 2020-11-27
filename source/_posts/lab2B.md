---
title: lab2B
date: 2020-11-09 10:19:13
tags:
category:
- 公开课
- mit_6.824
---


***<center> 实现LogReplication</center>***

## 客户端提交Command(实现Start)

任意的服务器接收来自客户端的日志追加请求，基本逻辑是检查自己是否为Leader,如果自己是Leader，则追加日志到自己的本地日志，并进行LogReplication。


## Leader向follower追加日志

Leader对follower[i]追加日志的逻辑: 在follower的Log数组里面找到与自己的Log完全相同（相同的index具有相同的Term）的一段Log，将follower与自己不同的后半段Log完全覆盖掉

1. Leader先检查nextIndex数组，查找nextIndex[i], nextIndex[i] - 1 表示该次AppendEntryRpc希望匹配到的Log的最后位置（完全相同的Log段的最后一个位置）
2. 对Follower，接收到AppendEntriesRpc请求，比较preLogIndex和preLogTerm，如果不同，则返回false，若相同，则追加后面的日志到自己的本机日志记录
3. 对Leader，接受到rpc回复，若成功，则更新follower[i]的nextIndex和matchIndex，若是由于日志不一致而导致的失败，则减少nextIndex并尝试重新提交
4. 对Leader, 如果存在N > commitIndex(本地待提交日志的索引)，majority(matchIndex[i]>=N),并且这些参与者索引为Nd的日志的任期也等于Leader的当前任期，更新Leader的commitIndex