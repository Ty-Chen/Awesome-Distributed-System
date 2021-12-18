分布式系统知识学习
====

分布式系统知识体系庞大而精妙，不花费大量的时间无法掌握。本文根据一些前人经验和自己摸索总结，由浅入深、由基础概念到实际运用，给出了一条学习曲线相对平滑的分布式学习攻略，希望和大家多多交流，共同进步。

1. 基本理论

2. 共识算法

3. 分布式锁

4. 分布式快照

5. 分布式时钟

6. 分布式错误检测

7. 链复制技术

8. 实际分布式系统的了解

9. 分布式运用

---

### 基本理论

* #### 入门级介绍
    入门级介绍包括一些对于初步接入分布式的同学们很有帮助的文献资料，可以让你从零开始了解分布式出现的原因，基本的理念，大致的发展过程，一些最基础的原则等等，很有参考价值，大致包括以下这些

  * [《Introduction To Distributed Systems》](https://pages.cs.wisc.edu/~zuyu/files/dist_systems.pdf) 本文是一篇启蒙式的文章，介绍了分布式系统方方面面的基本概念，适宜作为入门级的学习资料。
  
  * [《The 8 fallacies of distributed computing》](https://www.simpleorientedarchitecture.com/8-fallacies-of-distributed-systems/) 本文介绍的是对于分布式系统方面的一些错误的认知，不仅适用于分布式系统，对于其他网络领域也同样适用，非常值得一看。另外，发现了b站有转载一个[视频](https://www.bilibili.com/video/BV1ha4y1a7L7/?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2Mzg3MTM2MzIsImciOiJnWXBLRHlRdjZDWEdnSFRyIiwiaWF0IjoxNjM4NzEzMzMyLCJ1c2VySWQiOjB9.f5ixrNrsdMxZpnl0AGdno8Cw1II1SVydA3gAkeNA1ME)讲述这些误解，也可以看看。
  
  * [《a note no distributed computing》](https://doc.akka.io/docs/misc/smli_tr-94-29.pdf) 本文从分布式系统性能评估等角度出发，可以让你有一个全面的印象，也值得一看。

  * [《Distributed Systems for Fun and Profit》](http://book.mixu.net/distsys/) 一本很薄的小书，但是涵盖了分布式系统各方面的基础理论，由浅入深，并提供了很多参考文献供以深入学习，是很棒的一本书。

  * [《Notes on Distributed Systems for Young Bloods》](http://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/) 大佬对于分布式学习和新人入门的一些推荐及经验总结，值得一看。

---
* #### 拜占庭将军问题

   [拜占庭将军问题](https://en.wikipedia.org/wiki/Byzantine_fault)是分布式的经典入门问题，这个问题是莱斯利·兰波特（Leslie Lamport）于 1982 年提出用来解释一致性问题的一个虚构模型，并由该问题引出了分布式的重要理论：CAP、FLP和DLS。
   
   - [《The Byzantine Generals Problem》](https://dl.acm.org/doi/pdf/10.1145/3335772.3335936) 提出该问题的经典论文肯定是要先看一下的。
   - [《Dr.Dobb’s - The Byzantine Generals Problem》](https://www.drdobbs.com/cpp/the-byzantine-generals-problem/206904396) DR.Doob's网站有很多很棒的文章，建议收藏。该文章对拜占庭将军问题的来龙去脉讲解的非常细腻，值得一看。
   - [《Practical Byzantine Fault Tolerance》](https://www.usenix.org/legacy/publications/library/proceedings/osdi99/full_papers/castro/castro.ps) PBFT是第一个得到广泛应用的 BFT 算法。只要系统中有 2/3 的节点是正常工作的，则可以保证一致性。PBFT 算法包括三个阶段来达成共识：预准备（Pre-Prepare）、准备（Prepare）和提交（Commit）。

---

* #### CAP

  CAP是分布式中最基础的概念，指导了实际分布式系统中关于一致性、可用性和分区容忍性的抉择问题。先了解清楚CAP有利于后续学习中对于分布式系统设计思想的理解。关于CAP可以参考以下资料学习

  * 首先可以看看[《A plain english introduction to CAP Theorem》](http://ksat.me/a-plain-english-introduction-to-cap-theorem)一文，该文用一个绝妙的比喻通俗易懂的说明了CAP的基本原理
  * [《CAP Theorem: Revisited》](https://robertgreiner.com/cap-theorem-revisited/)一文通过绘图和简洁的讲解说明了为什么只有可能出现CP或者AP而不存在CA模型
  * [《The CAP FAQ》](https://www.the-paper-trail.org/page/cap-faq/)是Henry Robinson在github上发表的一系列FAQ，这里前排推荐这位大牛的博客，有很多不错的文章，如[《Distributed systems theory for the distributed systems engineer》](https://www.the-paper-trail.org/post/2014-08-09-distributed-systems-theory-for-the-distributed-systems-engineer/)
  * 最后可以看看论文[《CAP Twelve Years Later: How the "Rules" Have Changed》](https://sites.cs.ucsb.edu/~rich/class/cs293b-cloud/papers/brewer-cap.pdf)更加全面的了解CAP的进步和详细理念

---

* #### FLP Impossible
  FLP得名于三位作者：Fischer, Lynch and Patterson，是分布式领域的重要定理。其之所以重要性如此之高，是因为它的贡献和香浓定理一样，直接了当的给出了上限，使得后人不需要穷尽所能去求解不可能的解。其核心思想概括为：**异步通信领域，但凡存在进程失败，哪怕只失败一个，也没有任何算法能保证非失败进程达到一致性**。 该理论的证明较为难以理解，所以下面给出一篇比较容易理解的FLP Impossible解读，以及论文原文。可以先尝试看看论文，看不懂的话再结合博客进行研究。

  - [《A Brief Tour of FLP Impossibility》](https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/)本文是一篇关于FLP不可能定理的解读文章，写的深入浅出非常厉害。

  - [《Impossibility of Distributed Consensus with One Faulty Process》](https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf)本文是论文原文，值得细读。

---

* #### DLS

  DLS同样得名于作者，该论文给出了容错的上限，总结一下即
  - 在部分同步（partially synchronous）的网络环境中（即网络延迟有一定的上限，但我们无法事先知道上限是多少），协议可以容忍最多 1/3 的拜占庭故障（Byzantine fault）。
  - 在异步（asynchronous）的网络环境中，具有确定性质的协议无法容忍任何错误
  - 在同步（synchronous）网络环境中（即网络延迟有上限且上限是已知的），协议可以容忍 100% 的拜占庭故障，但当超过 1/2 的节点为恶意节点时，会有一些限制条件。要注意的是，我们考虑的是"具有认证特性的拜占庭模型（authenticated Byzantine）"，而不是"一般的拜占庭模型"；具有认证特性指的是将如今已经过大量研究且成本低廉的公私钥加密机制应用在我们的算法中。

  比较有价值的文献有
  
  - [《Consensus in the presence of partial synchrony》](https://dl.acm.org/doi/abs/10.1145/42282.42283)，即DLS原文，值得通篇阅读
  - [《On optimal probabilistic asynchronous Byzantine agreement》](https://link.springer.com/chapter/10.1007/978-3-540-77444-0_7)，基于 DLS基础上对异步网络环境的深入研究，在这种情况下可以容忍最多 1/3 的拜占庭故障。

---

* #### BASE和ACID
    BASE和ACID是两组相反的概念。
    - ACID是Atomicity, Consistency, Isolation, and Durability的首字母缩写，即原子性、一致性、隔离性和持久性。ACID是较为严格的要求，一般在关系型数据库中才会满足，是一个数据库是否支持事务的衡量标准。其最大的问题在于较高的要求会导致扩展性和灵活性下降，因此NoSQL一般不支持ACID，而是改为支持较为松散的BASE。
    - BASE是Basically Available, Soft-state, Eventually consistent的首字母缩写，即基本可用性，软状态，最终一致性。BASE不像ACID一样时刻保持一致性和可用性，而是会容忍一定程度的不可用、不一致的存在。这在很多场合下其实是可以满足需要的。放低要求也就带来了性能的提升，所以具体如何设计一个分布式系统需要结合实际场景、实际需求来做。可以参考此文[《Base: An Acid Alternative》](https://queue.acm.org/detail.cfm?id=1394128)深入了解。

---

* #### 最终一致性

   最终一致性来源于BASE，其中最出名的莫过于Dynamo的应用。
   - [《Dynamo: Amazon’s Highly Available Key-value Store》](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf) Dynamo是最终一致性的典型使用案例，该论文还有很多别的亮点，后面还会提到。
   - [《Life beyond Distributed Transactions: an Apostate’s Opinion》](https://www.ics.uci.edu/~cs223/papers/cidr07p15.pdf)这篇也是必看论文。

---

### 共识算法

* #### 两阶段提交和三阶段提交

    两阶段提交和三阶段提交是共识算法的基础方法，也是广泛使用的共识算法，可以参考以下文献了解。
    
    - [《Consensus Protocols: Two-Phase Commit》](https://www.the-paper-trail.org/post/2008-11-27-consensus-protocols-two-phase-commit/)
    - [《Consensus Protocols: Three-phase Commit》](https://www.the-paper-trail.org/post/2008-11-29-consensus-protocols-three-phase-commit/)

---

* #### Paxos
    Paxos算法是共识算法中最经典的算法，目前依然被大量的商业化软件使用。Paxos被公认较难理解，而且实现起来困难，因为作为一个纯学术理论的产物，设计伊始并未考虑工业界实现的难度。
  - [《A plain english introduction to CAP Theorem》](http://ksat.me/a-plain-english-introduction-to-cap-theorem)一文用一个通俗易懂的事例讲述了CAP原理，可以作为入门读物。
  - [《CAP Theorem: Revisited》](https://robertgreiner.com/cap-theorem-revisited/)一文通过绘图和简洁的讲解说明了为什么只有可能出现CP或者AP而不存在CA模型。
  - [《The CAP FAQ》](https://github.com/henryr/cap-faq)是Henry Robinson在github上发表的一系列FAQ，这里前排推荐这位大牛的博客，有很多不错的文章，如[《Distributed systems theory for the distributed systems engineer》](https://www.the-paper-trail.org/post/2014-08-09-distributed-systems-theory-for-the-distributed-systems-engineer/)。
---


* #### Raft
    Raft算法是Paxos的改良版，因为由工业界提出，其初衷即为便于实现，因此是一个叫较为容易实现的工业级共识算法。
   - 首先Raft的经典论文[《In Search of an Understandable Consensus Algorithm》](https://people.eecs.berkeley.edu/~kubitron/courses/cs262a-F18/handouts/papers/raft-technicalReport.pdf) 当然是要认真学习的
   - 然后相对于论文，有一个可视化的[算法演示网站](http://thesecretlivesofdata.com/raft/)也是不容错过的。

---


* #### Zab

    ZAB算法全称Zookeeper Atomic Broadcast，是Zookeeper中使用的共识算法，作为开源项目，广泛应用于Hadoop，HBase等之中。
    
    - 首先经典论文一定要学习，[《ZooKeeper’s atomic broadcast protocol: Theory and practice》](http://www.tcs.hut.fi/Studies/T-79.5001/reports/2012-deSouzaMedeiros.pdf)
    - 然后还有经典论文[《Zab: High-performance broadcast for primary-backup systems》](https://marcoserafini.github.io/papers/zab.pdf)
    - 雅虎的论文也不容错过[《A simple totally ordered broadcast protocol》](http://diyhpl.us/~bryan/papers2/distributed/distributed-systems/zab.totally-ordered-broadcast-protocol.2008.pdf)


---



### 分布式锁

   分布式锁，顾名思义是用于分布式系统的锁，其作用和多线程贡献资源时的锁本质上是一样的，只不过是在分布式的场景下，所以需要考虑更多的问题。这篇博客很值得一看[《How to do distributed locking》](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html)。除此之外还有[《Leases: an efficient fault-tolerant mechanism for distributed file cache consistency》](https://dl.acm.org/doi/10.1145/74851.74870)值得学习。

* #### Redis

    - 官方指导文档[《Distributed locks with Redis》](https://redis.io/topics/distlock)肯定是要先看的，这里给出了数十种不同的分布式锁的实现，可以选择自己擅长的语言进行学习。
    - [《Distributed Lock Implementation With Redis》](https://dzone.com/articles/distributed-lock-implementation-with-redis)讲述了实现Redis分布式锁的过程，也值得一看。

---

* #### Memcache

---

* #### Chubby

    - [《The Chubby lock service for loosely-coupled distributed systems》](https://static.googleusercontent.com/media/research.google.com/zh-CN//archive/chubby-osdi06.pdf)

---

* #### Zookeeper

    - [《 ZooKeeper: Distributed Process Coordination》](https://www.amazon.com/ZooKeeper-Distributed-Coordination-Flavio-Junqueira/dp/1449361307)

---

### 分布式快照

---


### 分布式时钟


* #### Lamport 时钟

---

* #### Vector 时钟

---


### 错误检测


* #### Gossip

---

### 链复制技术



---


### 实际分布式系统了解


* #### Dynamo

---

* #### GFS

---

* #### MapReduce

---

* #### Bigtable

---

* #### Chubby

---

* #### F1

---

* #### SPANNER

---

* #### ...

---

### 弹力设计


---
### 性能设计


---


---
### 实际运用

* #### 要实现数据副本的一致性，到底该选 Paxos 算法，还是 Raft 算法？

---

* #### 为什么我的集群接入性能低？稍微出现峰值流量，为什么业务就基本不可用了？

---

* #### 如何设计分布式系统架构呢？那么多算法，Paxos、Raft、Gossip、Nuorum NWR、PBFT 等等，究竟该选择哪个

---
