---
title: 面试题
taxonomy:
    category: docs
---

#### No.33：分布式 CAP 定理讲的是什么？如何推导？

分布式 CAP 定理是关于分布式系统设计中的三个重要属性的不可兼得性的定理。这三个属性分别是：一致性（Consistency）、可用性（Availability）和分区容错性（Partition tolerance）。

1. 一致性指的是在分布式系统中的所有节点上读取到的数据都是一致的。换句话说，当一个节点写入了新的数据后，其他节点必须能够立即读取到最新的数据。
2. 可用性指的是分布式系统在面对部分节点故障或网络分区时仍能保持可用状态，即系统仍然能够接受和处理客户端的请求。
3. 分区容错性是指分布式系统能够在面对网络分区（节点之间无法通信）的情况下依然保持正常运行。

CAP 定理指出，在一个分布式系统中，一致性、可用性和分区容错性这三个属性不可能同时满足。也就是说，在面对网络分区时，我们只能在一致性和可用性之间进行权衡选择。

根据分布式系统设计中的异步通信模型和 FLP 结果（FLP 结果是一个涉及异步系统的定理，表明在异步通信模型中，不存在一致性算法，可以保证在任意节点故障的情况下都能达到一致性）可以推导出 CAP 定理。

#### No.34：国产分布式数据库 TiDB 的技术原理是什么？

TiDB 是一种基于高性能键值（KV）磁盘数据库开发的 SQL 兼容分布式数据库，技术原理如下：

1. 分布式架构：TiDB 采用分布式架构，将数据存储和计算分散在多个节点上，实现数据的水平分割和负载均衡。它由三个核心组件组成：TiDB Server、TiKV 和 PD（Placement Driver）。
2. 分布式事务：TiDB 支持分布式事务处理，采用 Google Spanner 的 Percolator 扩展实现。它通过基于多版本并发控制（MVCC）来提供事务的隔离性，使用两阶段提交协议来保证事务的原子性和持久性。
3. 分布式一致性：TiDB 使用 Raft 一致性算法进行数据复制和强一致性保证。Raft 算法确保了数据在集群中各个节点之间的一致性，保证了数据的可靠性和可用性。
4. 分布式存储引擎：TiDB 的底层存储引擎是 TiKV（分布式 Key-Value 存储引擎），它是一个分布式、高可用、自动水平扩展的存储系统。TiKV 使用 Raft 算法进行数据复制，支持强一致性模型和事务操作。
5. 分布式查询优化：TiDB 使用分布式查询优化器来解析和优化 SQL 查询语句，并将查询计划分发给各个节点执行。通过并行处理和智能的数据分片策略，实现了高效的分布式查询操作。
6. 自动水平扩展：TiDB 支持自动水平扩展，可以根据负载情况和数据大小自动添加或删除节点，以提供更好的性能和可伸缩性。

#### No.35：国产分布式数据库 OceanBase 的技术原理是什么？

OceanBase 是一种基于数据分区的分布式数据库，和传统的分库分表/中间件技术比较类似，可以看做一种比较完善的高性能分库分表解决方案，技术原理如下：

1. 分布式架构：采用分布式架构，将数据划分为多个区（Partition），每个区可以存储和处理部分数据。不同的区可以分布在不同的服务器上，实现数据的分布存储和并行处理。
2. 高可靠性：通过数据冗余和容错机制来提供高可靠性。它采用了 Multi-Paxos 协议来实现数据的一致性和副本同步，当某个节点发生故障时，可以从其他副本节点中恢复数据。
3. 分布式事务：支持分布式事务，它使用两阶段提交（Two-Phase Commit）协议来实现分布式事务的原子性和一致性。同时，OceanBase还支持快照隔离级别和多版本并发控制（MVCC）等技术，提供高效的事务处理能力。
4. 水平扩展：可以根据需求进行水平扩展，通过增加节点来实现数据库的容量和性能的扩展。它提供了自动负载均衡和动态迁移数据的功能，保证数据在集群中的均衡存储和访问。
5. 多租户支持：支持多租户架构，可以将不同的租户的数据进行隔离和管理。每个租户可以拥有自己独立的数据库和资源配额，保证不同租户之间的数据安全和性能隔离。

#### No.36：TiDB 和 OceanBase 分别适合哪些场景？

TiDB 和 OceanBase 都是分布式关系型数据库，但是由于其底层采用了截然不同的两种架构，所以它们适用于不同的场景：

TiDB 适合以下场景：

1. 系统规模高速扩展：TiDB 采用水平扩展的方式，可以通过增加节点来提升系统的性能和容量。
2. 在线分析处理 OLAP：分布式 KV 数据库适合大规模数据读写，尤其是写入性能比读取性能更高。
3. 对一致性有强需求的场景：这是 TiDB 和关系型分布式数据库最大的不同，它可以保证全局强一致，为此甚至牺牲了可用性。

OceanBase 适合以下场景：

1. 分布式事务：OceanBase 支持分布式事务，可以保证多节点之间的数据一致性和事务的 ACID 特性。
2. 超高并发读写：作为世界性能第一的分布式数据库，这一点无需多言。