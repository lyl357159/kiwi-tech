# Raft

## 角色
  - **Leader**: 所有请求的处理者，接收客户端发起的操作请求，写入本地日志后同步至集群其它节点。
  - **Follower**: 请求的被动更新者，从 leader 接收更新请求，写入本地文件。如果客户端的操作请求发送给了 follower，会首先由 follower 重定向给 leader。
  - **Candidate**: 如果 follower 在一定时间内没有收到 leader 的心跳，则判断 leader 可能已经故障，此时启动 leader election 过程，本节点切换为 candidate 直到选主结束。
---
## 参考资料
  - [浅谈Paxos和Raft算法](https://blog.csdn.net/weixin_46263596/article/details/126734531)

---
- [回到首页](../../../README.md)