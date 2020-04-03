Distributed System Learning
===
This is a guide for new learner who is interested in distributed systems. Here not only including many good blogs or papers, but also gives a way for those who know nothing about distributed systems to grasp the core of distributed systems gradually.

---

1. Basic Theory

2. Consensus Theory

3. Distributed Lock

4. Distributed Snapshot

5. Distributed Clock

6. Failure Detector

7. Chain Replication

8. Real Distributed Systems

9. How to design distributed systems

---

### Basic Theory

* #### Introduction to distributed systems
Here are some good resources for new learners to have a basic eye on distributed system. Through these papers and blogs, you can learn many things simple but important about distributed systems.

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

### 
