# Paxos
 - Paxos属于共识算法，共识算法（Consensus Algorithm）就是用来做这个事情的，它保证即使在小部分（≤ (N-1)/2）节点故障的情况下，系统仍然能正常对外提供服务。共识算法通常基于状态复制机（Replicated State Machine）模型，也就是所有节点从同一个 state 出发，经过同样的操作 log，最终达到一致的 state。
-  Paxos算法在分布式领域具有非常重要的地位，可以说是Raft,Zab、微信的PhxPaxos协议的鼻祖。但是Paxos算法有两个比较明显的缺点： 
   - 1.难以理解 
   - 2.工程实现更难
   
## 角色
   - Paxos将系统中的角色分为提议者 (Proposer)，决策者 (Acceptor)，和最终决策学习者 (Learner):
   - **Proposer**: 提出提案 (Proposal)。Proposal信息包括提案编号 (Proposal ID) 和提议的值 (Value)。
   - **Acceptor**: 参与决策，回应Proposers的提案。收到Proposal后可以接受提案，若Proposal获得多数Acceptors的接受，则称该Proposal被批准。
   - **Learner**: 不参与决策，从Proposers/Acceptors学习最新达成一致的提案(Value)。


## 算法阶段
  - 第一阶段: Prepare阶段
    Proposer向Acceptors发出Prepare请求，Acceptors针对收到的Prepare请求进行Promise承诺。
    - Prepare: Proposer生成全局唯一且递增的Proposal ID (可使用时间戳加Server ID)，向所有Acceptors发送Prepare请求，这里无需携带提案内容，只携带Proposal ID即可。
    - Promise: Acceptors收到Prepare请求后，做出“两个承诺，一个应答”。
      - 承诺1: 不再接受Proposal ID小于等于(注意: 这里是<= )当前请求的Prepare请求;
      - 承诺2: 不再接受Proposal ID小于(注意: 这里是< )当前请求的Propose请求;
      - 应答: 不违背以前作出的承诺下，回复已经Accept过的提案中Proposal ID最大的那个提案的Value和Proposal ID，没有则返回空值。
   - 第二阶段: Accept阶段
     Proposer收到多数Acceptors承诺的Promise后，向Acceptors发出Propose请求，Acceptors针对收到的Propose请求进行Accept处理。
     - Propose: Proposer 收到多数Acceptors的Promise应答后，从应答中选择Proposal ID最大的提案的Value，作为本次要发起的提案。如果所有应答的提案Value均为空值，则可以自己随意决定提案Value。然后携带当前Proposal ID，向所有Acceptors发送Propose请求。
     - Accept: Acceptor收到Propose请求后，在不违背自己之前作出的承诺下，接受并持久化当前Proposal ID和提案Value。
   - 第三阶段: Learn阶段
     Proposer在收到多数Acceptors的Accept之后，标志着本次Accept成功，决议形成，将形成的决议发送给所有Learners。

---
## 参考资料
  - [浅谈Paxos和Raft算法](https://blog.csdn.net/weixin_46263596/article/details/126734531)

---
- [回到首页](../../../README.md)