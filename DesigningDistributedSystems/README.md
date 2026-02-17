# Designing Distributed Systems 精读笔记

> 作者：Brendan Burns (Microsoft Azure 分布式系统工程师)
>
> 原书位置：~/Downloads/Designing Distributed Systems.pdf

## 书籍简介

本书是 Brendan Burns 撰写的经典分布式系统著作，总结了三大类分布式系统模式：
1. **有状态模式** (Stateful Patterns)
2. **无状态模式** (Stateless Patterns)
3. **批处理模式** (Batch Patterns)

## 章节列表

| 章节 | 标题 | 主要内容 |
|-----|------|---------|
| 01 | [Introducing Distributed Systems](Chapter01_Introducing_深度版.md) | 分布式系统概述、挑战、CAP定理 |
| 02 | [Singleton Architectures](Chapter02_SingletonArchitectures_深度版.md) | 主-工作模式、分布式键值存储 |
| 03 | [Sharded Services](Chapter03_ShardedServices_深度版.md) | 分片策略、键选择、再平衡 |
| 04 | [Ownership and Election](Chapter04_OwnershipAndElection_深度版.md) | 领导选举、分布式锁、动态所有权 |
| 05 | [Request/Response](Chapter05_RequestResponse_深度版.md) | 同步/异步请求、负载均衡、故障处理 |
| 06 | [Event-Driven Architecture](Chapter06_EventDrivenArchitecture_深度版.md) | 发布订阅、事件溯源、CQRS |
| 07 | [Stateful Services and Event Sourcing](Chapter07_StatefulServicesAndEventSourcing_深度版.md) | 聚合根、状态管理、快照机制 |
| 08 | [Batch Computational Patterns](Chapter08_BatchComputationalPatterns_深度版.md) | MapReduce、工作流、数据管道 |
| 09 | [Work Queue Systems](Chapter09_WorkQueueSystems_深度版.md) | 任务队列、优先级队列、死信队列 |
| 10 | [Event-Driven Messaging](Chapter10_EventDrivenMessaging_深度版.md) | 消息路由、消息转换、事件处理 |
| 11 | [Distributed Messaging Patterns](Chapter11_DistributedMessagingPatterns_深度版.md) | 消息传递、可靠性、事务、Saga |
| 12 | [Service Aggregation](Chapter12_ServiceAggregation_深度版.md) | 同步/异步聚合、数据合并、响应聚合 |
| 13 | [API Gateway](Chapter13_APIGateway_深度版.md) | 请求路由、认证授权、限流熔断 |
| 14 | [Service Discovery](Chapter14_ServiceDiscovery_深度版.md) | 服务注册、健康检查、负载均衡 |
| 15 | [Circuit Breakers](Chapter15_CircuitBreakers_深度版.md) | 熔断器模式、降级策略、隔离限流 |
| 16 | [Conclusion](Chapter16_Conclusion_深度版.md) | 模式回顾、设计原则、未来趋势 |

## 核心模式框架

```
有状态模式                    无状态模式                    批处理模式
├─ Singleton                ├─ Request/Response           ├─ MapReduce
├─ Sharded Services         ├─ Service Aggregation        ├─ Work Queue
├─ Ownership/Election       ├─ API Gateway                ├─ Event Messaging
└─ Event-Driven             └─ Service Discovery          └─ Batch Pipeline
```

## 每章包含内容

- 核心概念和定义
- Mermaid 架构图和流程图
- 代码示例和伪代码
- 技术选型对比表
- 最佳实践建议
- 常见问题和解决方案

## 阅读建议

1. **入门**：先读第1章理解基础概念
2. **有状态服务**：第2-4章是核心
3. **通信模式**：第5-6章了解请求响应和事件驱动
4. **高级模式**：第7-8章深入有状态和批处理
5. **消息系统**：第9-11章掌握消息队列
6. **服务治理**：第12-15章学习网关、发现、熔断
7. **总结**：第16章回顾全部模式

---

*笔记生成完成*
