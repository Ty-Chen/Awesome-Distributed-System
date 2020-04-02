分布式系统知识学习
====

分布式系统知识体系庞大而精妙，不花费大量的时间无法掌握。这里是由浅入深、由基础概念到实际运用的分布式学习之路

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

  * [Introduction To Distributed Systems]()
  * [The 8 fallacies of distributed computing]()
  
  * [a note no distributed computing]()

---

* #### CAP

  CAP是分布式中最基础的概念，指导了实际分布式系统中关于一致性、可用性和分区容忍性的抉择问题。先了解清楚CAP有利于后续学习中对于分布式系统设计思想的理解。关于CAP可以参考以下资料学习

  * 首先可以看看[《A plain english introduction to CAP Theorem》](http://ksat.me/a-plain-english-introduction-to-cap-theorem)一文，该文用一个绝妙的比喻通俗易懂的说明了CAP的基本原理
  * [《CAP Theorem: Revisited》](https://robertgreiner.com/cap-theorem-revisited/)一文通过绘图和简洁的讲解说明了为什么只有可能出现CP或者AP而不存在CA模型
  * [《The CAP FAQ》](https://www.the-paper-trail.org/page/cap-faq/)是Henry Robinson在github上发表的一系列FAQ，这里前排推荐这位大牛的博客，有很多不错的文章，如[《Distributed systems theory for the distributed systems engineer》](https://www.the-paper-trail.org/post/2014-08-09-distributed-systems-theory-for-the-distributed-systems-engineer/)
  * 最后可以看看论文[《CAP Twelve Years Later: How the "Rules" Have Changed》](https://sites.cs.ucsb.edu/~rich/class/cs293b-cloud/papers/brewer-cap.pdf)更加全面的了解CAP的进步和详细理念

---

* #### FLP Impossible

---

* #### BASE

---

* #### ACID

---

### 共识算法

* #### Paxos

---


* #### Raft

---


* #### Zab

----

* 拜占庭错误是怎么回事？为什么区块链用拜占庭容错算法？Paxos 算法不行吗？能黑比特币吗？

---



### 分布式锁


* #### Redis

---

* #### Memcache

---

* #### Chubby

---

* #### Zookeeper

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

### 实际运用

* #### 要实现数据副本的一致性，到底该选 Paxos 算法，还是 Raft 算法？

---

* #### 为什么我的集群接入性能低？稍微出现峰值流量，为什么业务就基本不可用了？

---

* #### 如何设计分布式系统架构呢？那么多算法，Paxos、Raft、Gossip、Nuorum NWR、PBFT 等等，究竟该选择哪个

---
