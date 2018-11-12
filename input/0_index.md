## Introduction
## 介绍

I wanted a text that would bring together the ideas behind many of the more recent distributed systems - systems such as Amazon's Dynamo, Google's BigTable and MapReduce, Apache's Hadoop and so on.

亚马逊云的 Dynamo, 谷歌的 BigTable 和 MapReduce, 以及 Apache 的 Hadoop 等分布式服务，一直以来我想把这类分布式系统底层的基本原理结构化汇集为一系列教程。

In this text I've tried to provide a more accessible introduction to distributed systems. To me, that means two things: introducing the key concepts that you will need in order to [have a good time](https://www.google.com/search?q=super+cool+ski+instructor) reading more serious texts, and providing a narrative that covers things in enough detail that you get a gist of what's going on without getting stuck on details. It's 2013, you've got the Internet, and you can selectively read more about the topics you find most interesting.

本教程尽可能地使用一些用浅显易懂的表达方式给读者介绍分布式系统。对于我来说意味这要做两件事：
1. 介绍常见的基本概念，为读者深入了解更具有学术性的文章做铺垫。
2. 解释清楚分布式系统的主要内容，但不会让读者陷入具体服务技术的细节的困惑。

本教程只介绍分布式系统的宏观概念，但这也是学习分布式领域必经之路，对于当前你感兴趣的特定分布式系统，比如区块链主题可以在网上找资料针对性学习。

In my view, much of distributed programming is about dealing with the implications of two consequences of distribution:

在我看来，大多数分布式编程本质上就是做一下两件事：
- 系统间信息快速传递 (that information travels at the speed of light)
- 独立互斥事件独立地失败 (that independent things fail independently*)

In other words, that the core of distributed programming is dealing with distance (duh!) and having more than one thing (duh!). These constraints define a space of possible system designs, and my hope is that after reading this you'll have a better sense of how distance, time and consistency models interact.

换句话说，分布式编程的核心就是处理多机物理距离特性带来的问题和多消息事件机制。两个约束条件定义了一种存在物理距离系统设计服务，希望读者通读本教程后对分布式系统的距离、时间和一致性抽象概念有所感悟。

This text is focused on distributed programming and systems concepts you'll need to understand commercial systems in the data center. It would be madness to attempt to cover everything. You'll learn many key protocols and algorithms (covering, for example, many of the most cited papers in the discipline), including some new exciting ways to look at eventual consistency that haven't still made it into college textbooks - such as [CRDTs](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type) and the [CALM](http://bloom-lang.net/calm/) theorem.

本教程主要介绍分布式编程和系统概念，这些会对读者进一步了解分布式数据中心的商业系统有所帮助。同时需要强调的是不会覆盖分布式系统设计到的方方面面。通读本教程，你将会掌握很多关键的协议和算法（例如涵盖了本学科中应用最多的学术论文），包含一些让人眼前一亮的实现分布式最终一致性新观点，这些在大学教科书里边是没有提及到的，例如：CRDTs 和 CALM 理论

I hope you like it! If you want to say thanks, follow me on [Github](https://github.com/mixu/) (or [Twitter](http://twitter.com/mikitotakada)). And if you spot an error, [file a pull request on Github](https://github.com/mixu/distsysbook/issues).

衷心地希望你能喜欢本教程，如果你想表达谢意，请关注作者 [Github 账号](https://github.com/mixu/) 或 [Twitter 账号](https://twitter.com/mikitotakada)。

同时，这是译者 [Github 账号](https://github.com/wenweih) 和[个人主页](https://huangwenwei.com)。

---

# 1. 入门

[The first chapter](intro.html) covers distributed systems at a high level by introducing a number of important terms and concepts. It covers high level goals, such as scalability, availability, performance, latency and fault tolerance; how those are hard to achieve, and how abstractions and models as well as partitioning and replication come into play.

[第一章](intro.html) 通过介绍一系列重要的术语和概念从宏观引入分布式系统。介绍一些抽象的概念：例如可扩展性 (scalability)，可用性 (availability)，性能 (performance)，延迟 (latency) 和容错 (fault tolerance)；同时支持以上这些分布式特性难度较大;抽象、模型、分区 (partitioning) 和复制 (replication) 如何发挥作用。

# 2. 分层抽象化 Up and down the level of abstraction

[The second chapter](abstractions.html) dives deeper into abstractions and impossibility results. It starts with a Nietzsche quote, and then introduces system models and the many assumptions that are made in a typical system model. It then discusses the CAP theorem and summarizes the FLP impossibility result. It then turns to the implications of the CAP theorem, one of which is that one ought to explore other consistency models. A number of consistency models are then discussed.

[第二章](abstractions.html) 深入研究抽象模型和不可能的结果，以 Nietzsche 的格言为开始，然后介绍系统模型和特定系统模型的假设前提条件。讨论 CAP 理论并总结为什么 FLP 永不成立。接着继续介绍 CAP 理论得出的推论，其中的一个结论会引入一致性模型的探讨。

# 3. 时钟和排序 Time and order

A big part of understanding distributed systems is about understanding time and order.  To the extent that we fail to understand and model time, our systems will fail. [The third chapter](time.html) discusses time and order, and clocks as well as the various uses of time, order and clocks (such as vector clocks and failure detectors).

时间和排序是分布式系统重要模块，如果我们不理解和模拟分布式系统的时间，在某种程度上可以说你实现的系统注定会失败。在 [第三章](time.html) 探讨分布式系统中时间和事件顺序、时钟和各种时间的关系、时间排序和各种类型的时钟的关系（譬如矢量时钟和故障检测器）。

# 4. 复制：防止数据差异 preventing divergence

The [fourth chapter](replication.html) introduces the replication problem, and the two basic ways in which it can be performed. It turns out that most of the relevant characteristics can be discussed with just this simple characterization. Then, replication methods for maintaining single-copy consistency are discussed from the least fault tolerant (2PC) to Paxos.

[第四章](replication.html) 介绍分布式数据拷贝问题，以及两种可行的复制方式。It turns out that most of the relevant characteristics can be discussed with just this simple characterization，本章最后最小容错 (least fault tolerant 2PC) 和 Paxos 算法来讨论维护单拷贝 (single-copy) 一致性的复制方法。

# 5. 复制：接受数据差异 accepting divergence

The [fifth chapter](eventual.html) discussed replication with weak consistency guarantees. It introduces a basic reconciliation scenario, where partitioned replicas attempt to reach agreement. It then discusses Amazon's Dynamo as an example of a system design with weak consistency guarantees. Finally, two perspectives on disorderly programming are discussed: CRDTs and the CALM theorem.

[第五章](eventual.html) 讨论分布式系统弱一致性拷贝。首先引入一种基本的拷贝方案，分区副本尝试达成一致性。然后以亚马逊的 Dynamo 作为例子讨论如何设计弱一致性拷贝。最后讨论无序编程（disorderly programming）的两种观点：CRDTs 和 CALM 理论。

# 6. 附录

[附录](appendix.html) 提供了深入研究分布式系统的推荐阅读材料。

---

<p class="footnote">*: This is a [lie](http://en.wikipedia.org/wiki/Statistical_independence). [This post by Jay Kreps elaborates](http://blog.empathybox.com/post/19574936361/getting-real-about-distributed-system-reliability).
</p>
