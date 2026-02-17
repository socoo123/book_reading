# 第二章：Singleton Architectures

## 一、单例架构概述

### 1.1 单例架构定义

单例架构（Singleton Architecture）是一种最简单的有状态分布式模式：

```mermaid
graph TD
    A[单例架构] --> B[单个主节点]
    A --> C[多个工作节点]
    A --> D[任务分配]
    A --> E[状态管理]

    B --> B1[负责协调]
    B --> B2[管理状态]
    B --> C1[接收请求]

    C --> C1[执行任务]
    C --> C2[报告结果]

    style A fill:#ff9,stroke:#333
```

**核心特征**：
- **单一主节点**：系统中只有一个主节点负责任务分配和状态管理
- **多个工作节点**：工作节点并行执行任务
- **中心化协调**：主节点作为协调中心
- **状态集中**：状态信息集中在主节点

### 1.2 单例架构的适用场景

什么时候使用单例架构：

| 场景 | 描述 | 示例 |
|-----|------|-----|
| 协调服务 | 需要集中协调的服务 | 分布式锁管理器 |
| 配置管理 | 集中管理配置信息 | 配置中心 |
| 任务调度 | 任务需要串行或按序执行 | 任务调度器 |
| 状态存储 | 需要维护全局状态 | 分布式缓存主节点 |

```mermaid
graph TD
    A[选择单例架构] --> B{满足以下条件?}
    B -->|需要集中状态| C[适合]
    B -->|需要全局协调| C
    B -->|任务可并行| C
    B -->|读多写少| C

    B -->|状态分散| D[不适合]
    B -->|高写入负载| D
    B -->|需要高可用| D
```

## 二、主-工作模式

### 2.1 模式结构

主-工作模式（Master-Worker Pattern）是最基本的单例架构：

```mermaid
graph TD
    subgraph 主-工作模式
    Client1[客户端1] --> Master[主节点]
    Client2[客户端2] --> Master
    Client3[客户端3] --> Master

    Master --> Worker1[工作节点1]
    Master --> Worker2[工作节点2]
    Master --> Worker3[工作节点3]

    Worker1 --> TaskResult1[任务结果]
    Worker2 --> TaskResult2[任务结果]
    Worker3 --> TaskResult3[任务结果]

    TaskResult1 --> Master
    TaskResult2 --> Master
    TaskResult3 --> Master

    Master --> Client1
    Master --> Client2
    Master --> Client3
    end

    style Master fill:#ff9,stroke:#333
    style Worker1 fill:#9ff,stroke:#333
    style Worker2 fill:#9ff,stroke:#333
    style Worker3 fill:#9ff,stroke:#333
```

**工作流程**：
1. 客户端发送请求到主节点
2. 主节点将任务分配给工作节点
3. 工作节点执行任务并返回结果
4. 主节点汇总结果并返回给客户端

### 2.2 主节点职责

主节点的核心职责：

```mermaid
graph TD
    A[主节点职责] --> B[任务队列管理]
    A --> C[工作节点管理]
    A --> D[任务分配]
    A --> E[结果汇总]
    A --> F[故障处理]

    B --> B1[接收新任务]
    B --> B2[维护任务队列]

    C --> C1[监控工作节点]
    C --> C2[注册/注销节点]

    D --> D1[选择工作节点]
    D --> D2[分配任务]

    E --> E1[收集结果]
    E --> E2[返回客户端]

    F --> F1[检测故障]
    F --> F2[重新分配]

    style A fill:#ff9,stroke:#333
```

### 2.3 工作节点职责

工作节点的核心职责：

```mermaid
graph TD
    A[工作节点] --> B[请求任务]
    A --> C[执行任务]
    A --> D[返回结果]
    A --> E[心跳汇报]

    B --> B1[定期向主节点请求]
    B --> B2[接收分配的任务]

    C --> C1[处理业务逻辑]
    C --> C2[记录执行状态]

    style A fill:#9ff,stroke:#333
```

## 三、分布式键值存储示例

### 3.1 系统架构

使用主-工作模式实现分布式键值存储：

```mermaid
graph TD
    subgraph 分布式键值存储
    Client[客户端] --> Master[主节点]

    Master --> Worker1[工作节点1<br/>存储 a-h]
    Master --> Worker2[工作节点2<br/>存储 i-p]
    Master --> Worker3[工作节点3<br/>存储 q-z]

    Worker1 --> DB1[(KV数据库)]
    Worker2 --> DB2[(KV数据库)]
    Worker3 --> DB3[(KV数据库)]
    end

    style Master fill:#ff9,stroke:#333
    style Worker1 fill:#9ff,stroke:#333
    style Worker2 fill:#9ff,stroke:#333
    style Worker3 fill:#9ff,stroke:#333
```

### 3.2 数据分片策略

键值存储的数据分片策略：

```mermaid
graph TD
    A[键值对] --> B[计算哈希值]
    B --> C[哈希空间]

    C --> D[分片1: 0x0000-0x5555]
    C --> E[分片2: 0x5556-0xAAAA]
    C --> F[分片3: 0xAAAB-0xFFFF]

    D --> D1[key1, value1]
    E --> E1[key2, value2]
    F --> F1[key3, value3]

    style A fill:#ff9,stroke:#333
    style C fill:#9f9,stroke:#333
```

**分片算法**：
```python
def get_shard(key):
    hash_value = hash(key)
    shard_index = hash_value % num_shards
    return shard_index
```

### 3.3 请求处理流程

客户端请求的处理流程：

```mermaid
sequenceDiagram
    participant C as 客户端
    participant M as 主节点
    participant W as 工作节点

    C->>M: GET key1
    M->>M: 计算分片索引
    M->>W: 请求分片数据
    W->>W: 查找key1
    W->>M: 返回value1
    M->>C: 返回value1

    Note over M,W: 故障处理：<br/>如果工作节点无响应，<br/>主节点从副本恢复
```

## 四、使用工作队列的单例架构

### 4.1 工作队列模式

使用消息队列实现解耦的主-工作架构：

```mermaid
graph TD
    subgraph 工作队列架构
    Client1[客户端1] --> Queue1[任务队列]
    Client2[客户端2] --> Queue1

    Queue1 --> Worker1[工作节点1]
    Queue1 --> Worker2[工作节点2]
    Queue1 --> Worker3[工作节点3]

    Worker1 --> ResultQueue1[结果队列]
    Worker2 --> ResultQueue1
    Worker3 --> ResultQueue1

    ResultQueue1 --> Client1
    ResultQueue1 --> Client2
    end

    style Queue1 fill:#9f9,stroke:#333
    style ResultQueue1 fill:#9f9,stroke:#333
    style Worker1 fill:#9ff,stroke:#333
    style Worker2 fill:#9ff,stroke:#333
    style Worker3 fill:#9ff,stroke:#333
```

### 4.2 异步处理优势

工作队列模式的优势：

| 优势 | 描述 |
|-----|------|
| **解耦** | 生产者和消费者独立演进 |
| **削峰填谷** | 应对突发流量 |
| **负载均衡** | 自动分配任务到空闲节点 |
| **可靠性** | 消息持久化，故障不丢失 |
| **可扩展** | 动态增加工作节点 |

### 4.3 任务分配策略

主节点的多种任务分配策略：

```mermaid
graph TD
    A[任务分配策略] --> B[轮询<br/>Round Robin]
    A --> C[最少连接<br/>Least Connections]
    A --> D[基于负载<br/>Load Based]
    A --> E[亲和性<br/>Affinity]

    B --> B1[按顺序分配]
    B --> B2[简单公平]

    C --> C1[分配给连接数最少的]
    C --> C2[负载均衡]

    D --> D1[基于CPU/内存负载]
    D --> D2[智能分配]

    E --> E1[相同客户端路由到相同节点]
    E --> E2[利用缓存]

    style A fill:#ff9,stroke:#333
```

## 五、单例架构的优缺点

### 5.1 优点

```mermaid
graph TD
    A[单例架构优点] --> B[简单易实现]
    A --> C[状态集中管理]
    A --> D[易于调试]
    A --> E[协调简单]

    B --> B1[架构清晰]
    B --> B2[开发成本低]

    C --> C1[数据一致性好]
    C --> C2[事务处理简单]

    style A fill:#9ff,stroke:#333
```

### 5.2 缺点与限制

```mermaid
graph TD
    A[单例架构缺点] --> B[单点故障]
    A --> C[扩展受限]
    A --> D[性能瓶颈]
    A --> E[主节点压力大]

    B --> B1[主节点故障导致系统不可用]
    B --> B2[需要高可用方案]

    C --> C3[无法水平扩展主节点]
    C --> C4[写入能力受限]

    D --> D1[所有请求经过主节点]
    D --> D2[网络开销大]

    style A fill:#f96,stroke:#333
```

### 5.3 高可用方案

解决单点故障的方案：

```mermaid
graph TD
    A[高可用方案] --> B[主备模式]
    A --> C[热备选举]
    A --> D[分布式协调]

    B --> B1[主节点故障<br/>备节点自动切换]
    B --> B2[如：ZooKeeper<br/>etcd]

    C --> C1[使用共识算法]
    C --> C2[如：Raft<br/>Paxos]

    D --> D3[去中心化设计]
    D --> D4[如：Cassandra<br/> Dynamo]

    style A fill:#ff9,stroke:#333
    style B fill:#9ff,stroke:#333
    style C fill:#9f9,stroke:#333
```

## 六、总结与启示

核心要点：
- 单例架构是最简单的有状态分布式模式，适合需要集中协调的场景
- 主-工作模式中，主节点负责任务分配和状态管理
- 工作队列实现解耦的生产者-消费者模式
- 单例架构的主要问题是单点故障和扩展受限

---

*本章精读笔记完成*
