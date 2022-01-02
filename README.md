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
   
   * #### Memcache
   Memcache由于其实现，本身做分布式锁不是很合适的选择，但是正因为其简单，可以作为了解分布式锁的入门级尝试，如
   
   - [《A simple distributed lock with memcached》](https://bluxte.net/musings/2009/10/28/simple-distributed-lock-memcached/)
   - [《Distributed resource locking using memcached》](https://source.coveo.com/2014/12/29/distributed-resource-locking/)

---

* #### Redis

    - 官方指导文档[《Distributed locks with Redis》](https://redis.io/topics/distlock)肯定是要先看的，这里给出了数十种不同的分布式锁的实现，可以选择自己擅长的语言进行学习。
    - [《Distributed Lock Implementation With Redis》](https://dzone.com/articles/distributed-lock-implementation-with-redis)讲述了实现Redis分布式锁的过程，也值得一看。

---

* #### Chubby

    Chubby是谷歌分布式领域的重要产品，Chubby的设计初衷是为了解决分布式系统中的一致性问题，其中最常见的就是分布式系统的选主需求及一致性的数据存储。Chubby选择通过提供粗粒度锁服务的方式实现。

    - [《The Chubby lock service for loosely-coupled distributed systems》](https://static.googleusercontent.com/media/research.google.com/zh-CN//archive/chubby-osdi06.pdf)谷歌的经典介绍论文当然是必读的。

---

* #### Zookeeper

    Zookeeper是apache基金会下的一个软件项目，它为大型分布式计算提供开源的分布式配置服务、同步服务和命名注册，其中分布式锁也是一大功能。

    - 想完整了解zookeeper可以参看[《 ZooKeeper: Distributed Process Coordination》](https://www.amazon.com/ZooKeeper-Distributed-Coordination-Flavio-Junqueira/dp/1449361307)，本书讲解较为详细，但是需要一定的时间才可以看完。
    - 官网的介绍也可以一看[《ZooKeeper Recipes and Solutions》](https://zookeeper.apache.org/doc/r3.1.2/recipes.html#Shared+Locks)
    - [《Redis and Zookeeper for distributed lock》](https://www.fatalerrors.org/a/redis-and-zookeeper-for-distributed-lock.html)这篇文章对redis和zookeeper的方案进行了比较，也值得一看

---

### 分布式时钟

   在单台机器上，我们很容易的就可以对所有的进程、事务进行排序，因为我们有一个统一的标准：CPU时钟。而对于多台机器，则相对麻烦，因为难免会出现CPU时钟周期不相同、时间延迟、或者人为造成时间的修改等等。为此，在分布式系统中，不同机器通讯的先后顺序是一个比较麻烦的问题。
   
   总的来说，有几种解决方法：
   
   1.同步中央时钟：对多台机器进行同步处理，使其时钟保持一致，如NTP协议。但是这样带来了一些问题，首先需要保持同步则需要定期进行通信，从而增加了额外的时延和带宽消耗。另外，该方法仅针对网络拓扑稳定的环境适用。因为如果有机器不断地增加、减少，是无法保证每台机器CPU时钟周期相同，也无法保证时钟同步。
   
   2.局部排序：对每台机器单独进行排序，采取一些算法使其序列统一。如Lamport Clock，后续的vector clock，以及version clock等。

* #### Lamport 时钟

    - 经典论文[《Time, Clocks, and the Ordering of Events in a Distributed System》](https://lamport.azurewebsites.net/pubs/time-clocks.pdf)必读。
---

* #### Vector 时钟

    - [《Why Vector Clocks are Easy》](https://riak.com/why-vector-clocks-are-easy/)

---

### 分布式快照

   通常分布式快照指的是Chandy-Lamport分布式快照算法，用于再缺乏全局时钟或全局时钟不可靠的分布式系统中确定一个全局性的状态。在理解了时序问题后，不难理解想要在某一时刻获取分布式系统的全局快照是一件不容易的事情，在论文中作者给出了一个有趣的比方：想要用一张照片拍摄一群纷飞的鸟群弄清每一只的位置是不容易的，而如果用多张照片拼接可以做到这一点，但是如何拼接就是技术活了，这也就是Chandy-Lamport算法。
   - [《Distributed Snapshots: Determining Global States of a Distributed System》](https://www.microsoft.com/en-us/research/uploads/prod/2016/12/Determining-Global-States-of-a-Distributed-System.pdf)只用认真阅读看懂这篇经典论文就够了。

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
