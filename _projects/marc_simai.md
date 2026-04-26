---
layout: page
title: "MARC-in-SimAI — 分布式 AI 训练组播算法仿真实现"
description: 在阿里巴巴开源仿真平台 SimAI 中实现 MARC 硬件组播集合通信算法
img: assets/img/projects/marc_simai.jpg
importance: 3
category: research
---

**技术栈：** C++ · Python · NS-3 · SimAI · 分布式系统

---

## 项目背景

大规模 GPU 分布式训练中，AllReduce 等集合通信操作是网络带宽的核心瓶颈。现有主流方案（Ring AllReduce、Tree AllReduce）基于逐 GPU 单播，带宽利用率低，最高浪费 70–80%。本项目在阿里巴巴开源的 AI 训练专用仿真平台 [SimAI](https://dl.acm.org/doi/10.1145/3772356.3772425)（HotNets '24）中首次实现了 MARC（Multicast Adaptive Route Constructor）——一种利用环剥离启发式与前缀聚合方案的硬件组播集合通信算法，将发送方传输时间复杂度从 O(N) 降至 O(1)。

论文：[One to Many: Closing the Bandwidth Gap in AI Datacenters with Scalable Multicast](https://dl.acm.org/doi/10.1145/3772356.3772425)

---

## 我的贡献（主要开发者）

**数据面实现 — NS-3 内核扩展**

自定义 `MarcHeader` 协议头类，封装 32-bit 组播前缀和 8-bit 覆盖范围，使数据包携带组播路由元数据。在 NS-3 的 `IpForward` 函数中注入 MARC 拦截钩子，实现交换机级的 1-to-N 包复制与多端口同步转发，模拟硬件组播的 O(1) 传输效率。通过独立 CSMA 测试脚本验证数据面正确性。

**控制面实现 — 环剥离算法**

实现 C++ `MarcController` 类，在仿真初始化阶段解析 NS-3 拓扑，构建 128-GPU Spectrum-X Fat-Tree（含 80 个交换机）的邻接表图结构。执行基于 BFS 的同心环分层，再通过贪心策略由外向内逐层选取覆盖最多未连接子节点的父交换机，构建带宽高效的组播树。

**性能评估流水线**

设计独立 NS-3 基准测试脚本，在 100Gbps 链路上对比串行单播与 MARC 组播在 64KB–64MB 消息规模下的集合完成时间（CCT）；编写自动化扫参脚本和 Python 可视化代码生成对比图表。实验结果表明 MARC 具有显著的 O(1) vs O(N) 扩展性优势，对 512MB 消息的尾延迟可降低约 52%。

**集成与调试**

诊断并修复 SimAI 在 128-GPU 大规模拓扑初始化时的内存崩溃和拓扑生成器 bug，制定"混合验证策略"——控制面在全系统中验证正确性，数据面在独立脚本中验证性能——将完整仿真流程封装为可复现的实验环境。
