# 分布式系统知识学习（二）架构篇
本篇从分布式架构的设计模式讲起，后续涉及到弹力设计、管理设计、性能设计等方方面面，最后会涉及到成熟项目的各种架构经验。

---

## 设计模式

首先，需要重点推荐的是微软云平台 Azure 上的设计模式。 [Cloud Design Patterns ](https://docs.microsoft.com/en-us/azure/architecture/patterns/)，这个网站上罗列了分布式设计的各种设计模式，可以说是非常全面和完整。对于每一个模式都有详细的说明，并有对其优缺点的讨论，以及适用场景和不适用场景的说明，实在是一个非常不错的学习分布式设计模式的地方。其中有如下分类。

- [设计模式：可用性](https://docs.microsoft.com/en-us/azure/architecture/framework/resiliency/reliability-patterns)；
- [设计模式：数据管理](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/data-management)；
- [设计模式：设计和实现](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/design-implementation)；
- [设计模式：消息](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/messaging)；
- [设计模式：管理和监控](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/management-monitoring)；
- [设计模式：性能和扩展](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/performance-scalability)；
- [设计模式：系统弹力](https://docs.microsoft.com/en-us/azure/architecture/framework/resiliency/reliability-patterns)；
- [设计模式：安全](https://docs.microsoft.com/en-us/azure/architecture/framework/resiliency/reliability-patterns)。

此之外，还有其它的一些关于分布式系统设计模式的网站和相关资料。

- [AWS Cloud Pattern](http://en.clouddesignpattern.org/index.php/Main_Page) ，这里收集了 AWS 云平台的一些设计模式。
- [Design patterns for container-based distributed systems](https://research.google.com/pubs/archive/45406.pdf) ，这是 Google 给的一篇论文，其中描述了容器化下的分布式架构的设计模式。
- [Patterns for distributed systems ](https://www.slideshare.net/pagsousa/patterns-fro-distributed-systems)，这是一个 PPT，其中讲了一些分布式系统的架构模式，你可以顺着到 Google 里去搜索。



---

## 弹力设计

- [《4 Architecture Issues When Scaling Web Applications: Bottlenecks, Database, CPU, IO》](http://highscalability.com/blog/2014/5/12/4-architecture-issues-when-scaling-web-applications-bottlene.html) ，本文讲解了后端程序的主要性能指标，即响应时间和可伸缩性这两者如何能提高的解决方案，讨论了包括纵向和横向扩展，可伸缩架构、负载均衡、数据库的伸缩、CPU 密集型和 I/O 密集型程序的考量等。

- [《Scaling Stateful Objects》](http://ithare.com/scaling-stateful-objects/) ，这是一本叫《Development&Deployment of Multiplayer Online Games》书中一章内容的节选，讨论了有状态和无状态的节点如何伸缩的问题。虽然还没有写完，但是可以给你一些很不错的基本概念和想法。

- [Scale Up vs Scale Out: Hidden Costs](https://blog.codinghorror.com/scaling-up-vs-scaling-out-hidden-costs/) ，Coding Horror 上的一篇有趣的文章，详细分析了可伸缩性架构的不同扩展方案（横向扩展或纵向扩展）所带来的成本差异，帮助你更好地选择合理的扩展方案，可以看看。


- [Best Practices for Scaling Out](https://www.infoq.cn/article/hiXg6WRDjvNE0VNuwzPg) ，OpenShift 的一篇讨论 Scale out 最佳实践的文章。

- [Scalability Worst Practices](https://www.infoq.com/articles/scalability-worst-practices/) ，这篇文章讨论了一些最差实践，你需要小心避免。

- [Reddit: Lessons Learned From Mistakes Made Scaling To 1 Billion Pageviews A Month](http://highscalability.com/blog/2013/8/26/reddit-lessons-learned-from-mistakes-made-scaling-to-1-billi.html) ，Reddit 分享的一些关于系统扩展的经验教训。

- 下面是几篇关于自动化弹性伸缩的文章。
  - [Autoscaling Pinterest](https://medium.com/@Pinterest_Engineering/auto-scaling-pinterest-df1d2beb4d64)；
  - [Square: Autoscaling Based on Request Queuing](https://developer.squareup.com/blog/autoscaling-based-on-request-queuing/)；
  - [PayPal: Autoscaling Applications](https://medium.com/paypal-tech/autoscaling-applications-paypal-fb5bb9fdb821)；
  - [Trivago: Your Definite Guide For Autoscaling Jenkins](https://tech.trivago.com/post/autoscaling-jenkins-guide/)；
  - Scryer: Netflix’s Predictive Auto Scaling Engine。

---

## 管理设计

---

## 性能设计

---
