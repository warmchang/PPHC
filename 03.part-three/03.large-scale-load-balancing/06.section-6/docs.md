---
menu: 硬件厂商的软件设计
title: 殊途共归：硬件厂商的软件设计
taxonomy:
    category: docs
---

![](https://qn.lvwenhan.com/2023-01-08-16731890288560.jpg)
<center>图 6-7 H3C ComwareV7 操作系统架构图</center>

这张图中最重要的就是那根“细细的红线”：网络管理进程完全位于用户态。这正是 DPDK 思想的核心，传统的 UNIX 网络模型已无法应对单机 10G 的速度，因为需要读写内存的上下文切换的速度太慢了。为了实现更高性能，必须从底层网络数据处理流程上进行革命——抛弃操作系统提供的网络栈，直接在用户态接管所有网络流量。

为了提高软件稳定性，H3C 还在系统内开发了一些高可用技术，与我们负载均衡集群高可用的设计思想不谋而合，异曲同工：

1. 支持单播、组播和热迁移的高可靠 IPC (进程间通信) 技术：对应的是双机热备以及 OSPF/ECMP、VRRP 协议等。
2. 多进程主备选举：对应的是 Keepalived 集群选举。
3. 进程级内存备份 (实时热备进程的内存，用于快速故障恢复和主从切换)：对应的是 OSPF/ECMP 不丢包多活技术、服务器的内存镜像技术。

### 负载均衡集群无法单独支撑一百万 QPS

通过本文构建的这个 200/400G 负载均衡集群，配合多个应用网关以及后面海量的物理服务器，我们终于成功实现了一百万 QPS 的目标。然而，这只是 Web 服务层面的百万 QPS，在真实世界中，数据库才是那个**最难解决的单点**，接下来我们将会用 5 章的篇幅尝试解决这个问题。