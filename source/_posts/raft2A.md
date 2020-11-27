---
title: raft2A
date: 2020-11-05 14:56:26
tags:
category:
- 公开课
- mit_6.824
---

***<center>实验完成leaderElection部分</center>***

## 每个raft节点需要持有的状态

### 在所有机器需要持久化的状态

在rpc响应之前，需要更新稳定的存储介质

```go
    state       State    //服务器所处的角色(Paper中不存在)
    currentTerm int      //服务器最后知道的Term
    voteFor     int      //当前任期内收到本机投票的候选人id
    log         []Log   //日志条目


```

### 每个Raft节点上的易变状态

```go
    commitIndex  int    //已知的被提交的最大日志条目的索引值 从 0 开始递增
    lastApplied  int    //已被状态机执行的最大日志条目的索引值， 从 0 开始递增
```

### 在Leader上的易变状态

```go
    nextIndex   []int   //index 为每台server的标号，元素为下条发送到该sever的日志索引（初始化为Leader的最新一条日志的index + 1）
    matchIndex  []int   //已经复制到该服务器的的最新索引， 从0 开始递增

```

### 为了方便编程所添加的一些数据

```go
    voteCh chan bool    //通知有投票请求到达
    appendEntryCh chan bool //通知有添加日志请求到达

```

### Raft的状态转换

考虑的问题：

1. 状态的转换需要加锁吗，因为状态转换的时候这段代码会被包裹起来？ 

```go
func (rf *Raft) beCandidate() {
    rf.mu.Lock()
    rf.state = Candidate
    rf.currentTerm++
    rf.votedFor = rf.me
    rf.mu.Unlock()
    go  rf.startElection()
}

func (rf *Raft) beFollower(term int) {
    rf.state = Follower
    rf.votedFor = NULL
    rf.currentTerm = term
}

func (rf *Raft) beLeader() {
    if rf.state != Candidate {
        return
    }
    rf.state = Leader
    rf.nextIndex = make([]int, len(rf.peers))
    rf.matchIndex = make([]int, len(rf.peers))
    for i := 0; i < len(rf.nextIndex); i++ {
        rf.nextIndex[i] = rf.getLastLogIdx() + 1
    }
}
```

### 告知有rpc请求到达

```go
func send(ch chan bool) {
    select {
    case<-ch:
    default:
    }
    ch <- true
}
```

## 实现投票

### 应对投票请求

应对投票请求的逻辑

1. 如果candidate的Term大于自身，立马转换为follower(集群中)
2. agrg.Term < rf.currentTerm, 拒绝投票
3. 已经投过票，拒绝投票
4. candidate的log不是最新的，拒绝投票
5. 同意投票，立马转换自身角色，并通知voteCh


一点思考，如果args.Term == rf.Term 会产生什么情况呢

```go
func (rf *Raft) RequestVote(args *RequestVoteArgs, reply *RequestVoteReply) {
    // Your code here (2A, 2B).
    rf.mu.Lock()
    defer rf.mu.Unlock()
    if args.Term > rf.currentTerm {
        rf.beFollower(args.Term)
        send(rf.voteCh)
    }
    success := false
    if args.Term < rf.currentTerm{

    }else if rf.votedFor != NULL && rf.votedFor != args.CandidateId{

    }else if args.LastLogTerm < rf.getLastLogTerm() {

    }else if args.LastLogTerm == rf.getLastLogTerm() && args.LastLogIndex < rf.getLastLogIdx() {

    }else {
        rf.votedFor = args.CandidateId
        success = true
        rf.state = Follower
        send(rf.voteCh)
    }
    reply.Term = rf.currentTerm
    reply.VoteGranted = success
}


```

### candidate开始选举

收到rpc回复之后的处理逻辑

1. 集群中有peer的Term比自己大，立马切换状态为Follower
2. 收到有效票，判断是否得到了大多数的选票，如果是，立马转换为Leader

一些思考，

1. 如果已经得到了多数票，那么是否需要其他的请求投票协程停下来
2. 变成Follower之后，是否可能因为脑裂的问题，再次统计票数成为了Leader（比如集群中有一个Follower领先太多）

```go

func (rf *Raft) startElection() {
    rf.mu.Lock()
    //DPrintf("[%v] start Election at Term %d", )
    args := &RequestVoteArgs{
        Term : rf.currentTerm,
        CandidateId: rf.me,
        LastLogIndex : rf.getLastLogIdx(),
        LastLogTerm : rf.getLastLogTerm(),
    }
    var votes int32 = 1;
    for i := 0 ; i < len(rf.peers); i++ {
        if i == rf.me {
            continue
    }
        go func(peer int){
            reply := &RequestVoteReply{}
            if rf.sendRequestVote(peer, args, reply){
                rf.mu.Lock()
                defer rf.mu.Unlock()
                if reply.Term > rf.currentTerm {
                    rf.beFollower(reply.Term)
                    send(rf.voteCh)
                    return
                }
                if rf.state != Candidate {
                    return
                }
                if reply.VoteGranted {
                    atomic.AddInt32(&votes, 1)
                }
                if atomic.LoadInt32(&votes) > int32(len(rf.peers)) / 2{
                    rf.beLeader()
                    send(rf.voteCh)
                }
            }
        }(i)
    }
}


```








## 实现日志追加(心跳算是特殊的日志)

### 应对日志追加请求

### Leader追加日志

## Debug过程

### 产生了dataRace

先是肉眼debug， 看看加锁的地方哪里不对，遵循lab：

1. 在所有用到共享数据的地方加锁
2. 不要再阻塞的地方加锁
3. 尽量是把大锁，包裹一个连续的处理共享数据的过程

### 没有产生一个Leader
