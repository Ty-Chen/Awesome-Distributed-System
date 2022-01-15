分布式系统知识学习（一）基础篇
====

分布式系统知识体系庞大而精妙，不花费大量的时间无法掌握。本文根据一些前人经验和自己摸索总结，由浅入深、由基础概念到实际运用，给出了一条学习曲线相对平滑的分布式学习攻略，希望和大家多多交流，共同进步。本篇为基础篇学习，涉及到了数百篇论文及博客资料，需要耐下心慢慢学习才可以体会到其精髓，欲速则不达。

1. 基本理论

2. 共识算法

3. 分布式锁

4. 分布式时钟

5. 分布式快照

6. 分布式错误检测

7. 链复制技术

8. 实际分布式系统

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
    - 分布式事务是一个全新的概念，结合Google App Engine 联合创始人瑞恩·巴雷特（Ryan Barrett）在 2009 年的 Google I/O 大会上的演讲[《Transaction Across DataCenter》](https://snarfed.org/transactions_across_datacenters_io.html)（[YouTube 视频](https://www.youtube.com/watch?v=srOgpXECblk)）可以有更好的理解。

---

* #### 最终一致性

   - [《Eventually Consistent - Revisited》   ](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)，这篇文章是 AWS 的 CTO 维尔纳·沃格尔（Werner Vogels）在 2008 年发布在 ACM Queue 上的一篇数据库方面的重要文章，阐述了 NoSQL 数据库的理论基石——最终一致性，对传统的关系型数据库（ACID，Transaction）做了较好的补充。
   - 最终一致性来源于BASE，其中最出名的莫过于Dynamo的应用。[《Dynamo: Amazon’s Highly Available Key-value Store》](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf) Dynamo是最终一致性的典型使用案例，该论文还有很多别的亮点，后面还会提到。
   - [《Life beyond Distributed Transactions: an Apostate’s Opinion》](https://www.ics.uci.edu/~cs223/papers/cidr07p15.pdf)这篇也是必看论文。

---

* #### 消息通知

   分布式的消息通知通常采用广播传输，常见的方式有三种：原子广播、Gossip算法以及随机广播。
   
   - [原子广播](https://en.wikipedia.org/wiki/Atomic_broadcast)：在该广播中，分布式系统中的所有正确进程都以相同的顺序接收相同的消息集。广播被称为“原子”，因为它要么最终在所有参与者正确完成，或者所有的参与者放弃无副作用。[《Total Order Broadcast and Multicast Algorithms:Taxonomy and Survey》](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.3.4709&rep=rep1&type=pdf)
   - [Gossip算法](https://en.wikipedia.org/wiki/Gossip_protocol)是分布式领域经典传输算法，经典论文为[《Epidemic Algorithms for Replicated . Database Maintenance》](http://bitsavers.informatik.uni-stuttgart.de/pdf/xerox/parc/techReports/CSL-89-1_Epidemic_Algorithms_for_Replicated_Database_Maintenance.pdf)。除此之外，[《Gossip Algorithms: Design, Analysis and Applications》](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.85.6176&rep=rep1&type=pdf)也非常值得一看。还有一个[动图网站](https://rrmoelker.github.io/gossip-visualization/)也很棒。
   - 随机广播来自于经典论文[《Lightweight Causal and Atomic Group Multicast》](https://www.cs.cornell.edu/courses/cs614/2003sp/papers/BSS91.pdf)，除此之外还有两篇对此深度思考及优化的论文很值得一读[《Understanding the Limitations of Causally and Totally Ordered Communication》](https://www.cs.rice.edu/~alc/comp520/papers/Cheriton_Skeen.pdf)、[《A Response to Cheriton and Skeen's Criticism of Causal and Totally Ordered Communication》](https://www.cs.princeton.edu/courses/archive/fall07/cos518/papers/catocs-limits-response.pdf)。

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

    Vector时钟是基于Lamport时钟的改进版本，旨在解决同时发生的事件的情况。

    - [《Why Vector Clocks are Easy》](https://riak.com/why-vector-clocks-are-easy/)这篇博客值得一读
    - [wiki](https://en.wikipedia.org/wiki/Vector_clock)写得很好，值得一看

---

* #### 分布式时钟同步综述

    学习完Lamport时钟和Vector时钟后，可以看一下这篇综述，算是对分布式时钟问题的总结。[《An Overview of Clock Synchronization》](https://www.researchgate.net/profile/Barbara-Simons/publication/221655803_An_Overview_of_Clock_Synchronization/links/0f31753aa09bbbfaa6000000/An-Overview-of-Clock-Synchronization.pdf)。

---

### 分布式快照

   通常分布式快照指的是Chandy-Lamport分布式快照算法，用于再缺乏全局时钟或全局时钟不可靠的分布式系统中确定一个全局性的状态。在理解了时序问题后，不难理解想要在某一时刻获取分布式系统的全局快照是一件不容易的事情，在论文中作者给出了一个有趣的比方：想要用一张照片拍摄一群纷飞的鸟群弄清每一只的位置是不容易的，而如果用多张照片拼接可以做到这一点，但是如何拼接就是技术活了，这也就是Chandy-Lamport算法。
   - [《Distributed Snapshots: Determining Global States of a Distributed System》](https://www.microsoft.com/en-us/research/uploads/prod/2016/12/Determining-Global-States-of-a-Distributed-System.pdf)只用认真阅读看懂这篇经典论文就够了。

---

### 错误检测

   错误检测的难点在于准确度和覆盖度的权衡，以下资料值得学习。
    
   - [《Unreliable Failure Detectors for Reliable Distributed Systems》](http://courses.csail.mit.edu/6.852/08/papers/CT96-JACM.pdf)这篇文章是分布式错误检测的经典论文，文章较长，可以选看
   - [《Survey on Scalable Failure Detectors》](http://www.scs.stanford.edu/14au-cs244b/labs/projects/song.pdf)这篇综述性的文章会比上文简短很多，建议学习。

---

### 链复制技术

   链复制技术将分布式系统视为虚拟的链表，并通过一定的方式进行顺序读写从而保障了数据的一致性。
   
   - 经典论文当然必读[《Chain Replication for Supporting High Throughput and Availability》](http://www.cs.cornell.edu/home/rvr/papers/OSDI04.pdf)
   - 普林斯顿大佬基于基础理论进行了实践，并对算法进行了优化，值得学习和思考：[《Object Storage on CRAQ：High-throughput chain replication for read-mostly workloads》](https://www.usenix.org/legacy/event/usenix09/tech/full_papers/terrace/terrace.pdf)。本文的阅读重点应放在学习大佬如何思考、优化、解决问题的思维方式上。
   - 另外还有一篇优化向的文章[《Chain Replication in Theory and in Practice》](http://diyhpl.us/~bryan/papers2/distributed/distributed-systems/chain-replication-in-theory-and-in-practice.2010.pdf)也很值得学习。

---


### 实际分布式系统

   实际分布式系统会综合采用上面的某些技术而实现，需要注意的是，**设计架构是为了满足需求，合适的才是最好的**。在学习以下实际分布式系统的时候，应该先了解他们遇到了什么问题，有哪些需求，因此设计了怎样的分布式系统进行解决，解决中又遇到了什么问题，最终如何得到满意的结果。带着思考去阅读，然后边思考边总结，才能学到分布式系统的精髓。

* #### Dynamo

    - [《Dynamo: Amazon’s Highly Available Key-value Store》](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)经典之中的经典，必读。

---

* #### GFS

    - [《The Google File System》](http://static.googleusercontent.com/media/research.google.com/en/us/archive/gfs-sosp2003.pdf)大名鼎鼎的谷歌文件系统，三驾马车之一。

---

* #### MapReduce

   - [《MapReduce: Simplified Data Processing on Large Clusters》](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.324.78&rep=rep1&type=pdf)谷歌的大数据处理方案，三驾马车之一。

---

* #### Bigtable

   - [《Bigtable: A Distributed Storage System for Structured Data》](http://static.googleusercontent.com/media/research.google.com/en/us/archive/bigtable-osdi06.pdf)谷歌的大数据存储方案，三驾马车之一。

---

* #### Chubby

  - 谷歌分布式锁，在上文分布式锁一节有所介绍。[《The Chubby lock service for loosely-coupled distributed systems》](http://static.googleusercontent.com/media/research.google.com/en/us/archive/chubby-osdi06.pdf)

---

* #### SPANNER

  - 谷歌的分布式数据库。Spanner 的扩展性达到了令人咋舌的全球级，可以扩展到数百万台机器，数以百计的数据中心，上万亿的行。更给力的是，除了夸张的扩展性之外，它还能同时通过同步复制和多版本来满足外部一致性，可用性也是很好的。[《Spanner: Google’s Globally-Distributed Database》](http://static.googleusercontent.com/media/research.google.com/en/us/archive/spanner-osdi2012.pdf)。另外多提一句，基于 Spanner 论文的开源实现有两个，一个是 Google 公司自己的人出来做的CockroachDB，另一个是国人做的TiDB。

---

* #### F1

   - F1是Google开发的分布式关系型数据库，主要服务于Google的广告系统.Google的广告系统以前使用MySQL，广告系统的用户经常需要使用复杂的query和join操作，这就需要设计shard规则时格外注意，尽量将相关数据shard到同一台MySQL上。扩容时对数据reshard时也需要尽量保证这一点，广告系统扩容比较艰难。在可用性方面老的广告系统做的也不够，尤其是整个数据中心挂掉的情况，部分服务将不可用或者丢数据。对于广告系统来说，短暂的宕机服务不可用将带来重大的损失。为了解决扩容/高可用的问题，谷歌的专家们设计了基于Spanner的跨数据中心的分布式关系型数据库，支持ACID，支持全局索引。[《F1: A Distributed SQL Database That Scales》](http://static.googleusercontent.com/media/research.google.com/en/us/pubs/archive/41344.pdf)

---

* #### MillWheel

   - MillWheel是谷歌内部使用的数据处理架构，[《MillWheel: Fault-Tolerant Stream Processing at Internet Scale》](http://static.googleusercontent.com/media/research.google.com/en/us/pubs/archive/41378.pdf)

---

* #### Omega

   - 容器及自动化运行随着docker和kubernetes火爆全球，但是谷歌行动的却要更早，只是一直内部使用对外未公开。谷歌先后开发了Borg, Omega和Kubernetes用以进行大规模容器管理，都有其值得学习借鉴之处。[《Omega: flexible, scalable schedulers for large compute clusters》](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/41684.pdf)
   - 《Borg, Omega, and Kubernetes》可以了解三者中的差异。


---

* #### Dapper

   - [《Dapper, a Large-Scale Distributed Systems Tracing Infrastructure》](http://static.googleusercontent.com/media/research.google.com/en/us/pubs/archive/36356.pdf)。

---

* #### Dryad

    - 微软的分布式并行计算框架，[官网](https://www.microsoft.com/en-us/research/project/dryad/)有简单的介绍，论文也值得学习 [《Dryad: distributed data-parallel programs from sequential building blocks》](https://cse.buffalo.edu/~stevko/courses/cse704/fall10/papers/eurosys07.pdf)。

---

* #### Cassandra

    - Apache Cassandra是一套开源分布式NoSQL数据库系统。它最初由Facebook开发，用于改善电子邮件系统的搜索性能的简单格式数据，集Google BigTable的数据模型与Amazon Dynamo的完全分布式架构于一身。[《Cassandra - A Decentralized Structured Storage System》](https://www.cs.cornell.edu/projects/ladis2009/papers/lakshman-ladis2009.pdf)。

---

* #### Ceph

    - Ceph是一个开源的分布式对象，块和文件存储。Ceph已经与Linux内核KVM集成，并且默认包含在许多GNU / Linux发行版中。[《Ceph: A Scalable, High-Performance Distributed File System》](https://www.usenix.org/legacy/events/osdi06/tech/full_papers/weil/weil_html/)。
    - Ceph的算法来源于另一篇文章：[《CRUSH: Controlled, Scalable, Decentralized Placement of Replicated Data》](https://www.ssrc.ucsc.edu/media/pubs/9c7bcd06ff4eeccef2cb4c7813fe33ba7d4805c7.pdf)。

---

* #### RAMCloud

   - RamCloud是一种适用于大规模数据中心应用的高速存储，[《The RAMCloud Storage System》](https://dl.acm.org/doi/pdf/10.1145/2806887)。

---

* #### HyperDex

   - HyperDex是一种NoSQL键值存储的分布式数据存储架构， [《HyperDex: A Distributed, Searchable Key-Value Store》](https://www.cs.cornell.edu/people/egs/papers/hyperdex-sigcomm.pdf)。

---

* #### PNUTS

   - Yahoo!的PNUTS是一个分布式的数据存储平台，它是Yahoo!云计算平台重要的一部分，该架构考虑到大部分web应用对一致性并不要求非常严格，在设计上放弃了对强一致性的追求。代替的是追求更高的availability，容错，更快速的响应调用请求等。。[《PNUTS: Yahoo!’s Hosted Data Serving Platform》](https://people.mpi-sws.org/~druschel/courses/ds/papers/cooper-pnuts.pdf)。

---

* #### Azure Data Lake Store

   - 来自微软的数据存储、分析架构，官方定义为：大规模可缩放且安全的数据湖，适用于高性能分析工作负载。[《Azure Data Lake Store: A Hyperscale Distributed File Service for Big Data Analytics》](https://dl.acm.org/doi/pdf/10.1145/3035918.3056100)

---

* #### AWS Aurora

   - Aurora 是 AWS 将 MySQL 的计算和存储分离后，计算节点 scale up，存储节点 scale out。并把其 redo log 独立设计成一个存储服务，把分布式的数据方面的东西全部甩给了底层存储系统。从而提高了整体的吞吐量和水平的扩展能力。[《Amazon Aurora: Design Considerations for High Throughput Cloud-Native Relational Databases》](https://www.allthingsdistributed.com/files/p1041-verbitski.pdf)。

---

* #### Kafka

    - 分布式消息系统的代表之作[《Kafka: a Distributed Messaging System for Log Processing》](http://notes.stephenholiday.com/Kafka.pdf)。

---

* #### Wormhole

    - Wormhole是 Facebook 内部使用的一个 Pub-Sub 系统，目前还没有开源。它和 Kafka 之类的消息中间件很类似。但是它又不像其它的 Pub-Sub 系统，Wormhole 没有自己的存储来保存消息，它也不需要数据源在原有的更新路径上去插入一个操作来发送消息，是非侵入式的。其直接部署在数据源的机器上并直接扫描数据源的 transaction logs，这样还带来一个好处，Wormhole 本身不需要做任何地域复制（geo-replication）策略，只需要依赖于数据源的 geo-replication 策略即可。[《Wormhole: Reliable Pub-Sub to Support Geo-replicated Internet Services》](https://www.usenix.org/system/files/conference/nsdi15/nsdi15-paper-sharma.pdf)。

---

* #### LinkedIn

    - [《All Aboard the Databus! LinkedIn’s Scalable Consistent Change Data Capture Platform》 ](https://engineering.linkedin.com/research/2012/all-aboard-the-databus-linkedlns-scalable-consistent-change-data-capture-platform)， 在 LinkedIn 投稿 SOCC 2012 的这篇论文中，指出支持对不同数据源的抽取，允许不同数据源抽取器的开发和接入，只需该抽取器遵循设计规范即可。该规范的一个重要方面就是每个数据变化都必须被一个单调递增的数字标注（SCN），用于同步。这其中的一些方法完全可以用做异地双活的系统架构中。



