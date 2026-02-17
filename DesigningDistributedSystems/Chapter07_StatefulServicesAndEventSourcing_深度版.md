# 第七章：Stateful Services and Event Sourcing

## 一、有状态服务概述

### 1.1 有状态服务定义

有状态服务（Stateful Service）维护和管理应用状态：

```mermaid
graph TD
    A[有状态服务] --> B[状态存储]
    A --> C[状态变更]
    A --> D[状态查询]
    A --> E[状态恢复]

    subgraph 状态生命周期
    S1[初始状态] --> S2[状态变更] --> S3[持久化] --> S4[状态恢复]
    end

    style A fill:#ff9,stroke:#333
    style S1 fill:#9ff,stroke:#333
    style S2 fill:#9f9,stroke:#333
    style S3 fill:#f96,stroke:#333
```

**核心特征**：
- **状态保持**：服务维护客户端会话信息
- **状态依赖**：处理请求需要访问状态
- **状态持久化**：状态需要持久化存储

### 1.2 有状态 vs 无状态

```mermaid
graph TD
    A[服务类型对比] --> B[有状态服务]
    A --> C[无状态服务]

    B --> B1[维护会话状态]
    B --> B2[请求依赖状态]
    B --> B3[扩展复杂]
    B --> B4[故障恢复困难]

    C --> C1[每次请求独立]
    C --> C2[状态外置]
    C --> C3[易于扩展]
    C --> C4[故障恢复简单]

    style B fill:#f96,stroke:#333
    style C fill:#9f9,stroke:#333
```

| 特性 | 有状态服务 | 无状态服务 |
|-----|-----------|-----------|
| **状态管理** | 服务内维护 | 外部存储 |
| **扩展性** | 受限 | 高 |
| **故障恢复** | 复杂 | 简单 |
| **性能** | 低延迟 | 需要状态获取 |
| **适用场景** | 会话、游戏 | Web API、批处理 |

## 二、状态管理策略

### 2.1 状态存储位置

```mermaid
graph TD
    A[状态存储位置] --> B[内存存储]
    A --> C[本地磁盘]
    A --> D[分布式缓存]
    A --> E[数据库]

    B --> B1[最快]
    B --> B2[易丢失]

    C --> C1[持久化]
    C --> C2[单点故障]

    D --> D1[平衡速度和持久]
    D --> D2[如Redis]

    E --> E1[强一致]
    E --> E2[如MySQL]

    style A fill:#ff9,stroke:#333
```

### 2.2 状态同步策略

```mermaid
graph TD
    A[状态同步] --> B[同步复制]
    A --> C[异步复制]
    A --> D[共识算法]

    B --> B1[强一致]
    B --> B2[延迟高]

    C --> C1[高性能]
    C --> C2[可能丢数据]

    D --> D1[如Raft]
    D --> D2[平衡一致性和性能]

    style A fill:#ff9,stroke:#333
```

### 2.3 状态分区策略

```mermaid
graph TD
    A[状态分区] --> B[按用户分区]
    A --> C[按功能分区]
    A --> D[按时间分区]

    B --> B1[用户数据在同一节点]
    B --> B2[用户体验好]

    C --> C1[不同功能独立扩展]
    C --> C2[管理复杂]

    D --> D1[时间序列数据]
    D --> D2[便于归档]

    style A fill:#ff9,stroke:#333
```

## 三、事件溯源深入

### 3.1 事件溯源架构

```mermaid
graph TD
    subgraph 事件溯源完整架构
    Command[命令] --> AG[聚合根]
    AG -->|产生| Events[事件流]

    Events --> Store[事件存储]
    Events --> Bus[事件总线]

    Store --> Snapshot[快照]
    Store --> Projection[投影]

    Bus --> H1[处理器1]
    Bus --> H2[处理器2]

    Projection --> ReadDB[读模型数据库]
    ReadDB --> API[查询API]
    end

    style Store fill:#f96,stroke:#333
    style ReadDB fill:#9ff,stroke:#333
```

### 3.2 事件结构

```mermaid
graph TD
    A[事件结构] --> B[元数据]
    A --> C[业务数据]

    B --> B1[事件ID]
    B --> B2[时间戳]
    B --> B3[聚合根ID]
    B --> B4[版本号]
    B --> B5[事件类型]

    C --> C1[状态变更前]
    C --> C2[状态变更后]
    C --> C3[变更原因]

    style A fill:#ff9,stroke:#333
```

### 3.3 快照机制

```mermaid
sequenceDiagram
    participant S as 快照存储
    participant E as 事件存储
    participant P as 处理器

    Note over P: 事件数量达到阈值
    P->>P: 读取所有事件
    P->>P: 重放构建状态
    P->>S: 保存快照

    Note over P: 下次恢复时
    P->>S: 获取最新快照
    P->>E: 获取快照后事件
    P->>P: 重放增量事件
    P->>P: 恢复完整状态

    style S fill:#9ff,stroke:#333
    style E fill:#f96,stroke:#333
```

### 3.4 投影构建

```mermaid
graph TD
    subgraph 投影构建
    Event[事件] --> Handler[事件处理]

    Handler --> Projector[投影器]
    Projector --> ReadModel[读模型]

    E1[事件1] --> H1
    E2[事件2] --> H2
    E3[事件3] --> H3

    H1 --> RM1[用户视图]
    H2 --> RM2[订单视图]
    H3 --> RM3[统计视图]
    end

    style ReadModel fill:#9ff,stroke:#333
```

## 四、聚合根模式

### 4.1 聚合根概念

聚合根（Aggregate Root）是聚合的入口点：

```mermaid
graph TD
    subgraph 聚合根
    AR[订单聚合根] --> OrderInfo[订单信息]
    AR --> Items[订单项]
    AR --> Payments[支付记录]

    OrderInfo --> ID[订单ID]
    OrderInfo --> Status[状态]
    OrderInfo --> Total[总价]

    Items --> Item1[商品1]
    Items --> Item2[商品2]

    Payments --> P1[支付1]
    end

    style AR fill:#ff9,stroke:#333
```

### 4.2 聚合边界

```mermaid
graph TD
    A[聚合设计原则] --> B[事务边界]
    A --> C[一致性边界]
    A --> D[最小化]

    B --> B1[一个聚合一个事务]
    B --> B2[避免跨聚合事务]

    C --> C1[聚合内强一致]
    C --> C2[聚合间最终一致]

    D --> D1[只包含必要实体]
    D --> D2[减少复杂度]

    style A fill:#ff9,stroke:#333
```

### 4.3 聚合间通信

```mermaid
sequenceDiagram
    participant AR1 as 订单聚合根
    participant EB as 事件总线
    participant AR2 as 库存聚合根

    AR1->>AR1: 处理订单创建
    AR1->>AR1: 验证库存

    Note over AR1: 不能直接操作库存聚合根

    AR1->>AR1: 发布库存锁定事件
    AR1->>EB: 发布事件

    EB->>AR2: 推送事件
    AR2->>AR2: 处理锁定库存

    Note over AR1,AR2: 通过事件实现最终一致性
```

## 五、状态恢复与容错

### 5.1 状态恢复流程

```mermaid
flowchart TD
    A[启动服务] --> B[检查本地状态]
    B --> C{有快照?}
    C -->|是| D[加载快照]
    C -->|否| E[从头重放]

    D --> F[获取后续事件]
    E --> F

    F --> G[重放事件]
    G --> H[构建当前状态]

    H --> I[开始处理请求]

    style A fill:#9ff,stroke:#333
    style I fill:#9f9,stroke:#333
```

### 5.2 故障恢复策略

```mermaid
graph TD
    A[故障恢复] --> B[主动复制]
    A --> C[被动备份]
    A --> D[多活架构]

    B --> B1[实时同步]
    B --> B2[故障切换快]

    C --> C1[定期备份]
    C --> C2[恢复时间长]

    D --> D1[多节点活跃]
    D --> D2[成本高复杂]

    style A fill:#ff9,stroke:#333
```

### 5.3 检查点机制

```mermaid
sequenceDiagram
    participant P as 处理进程
    participant E as 事件存储
    participant C as 检查点存储

    loop 处理事件
        P->>E: 读取事件
        E-->>P: 返回事件
        P->>P: 处理事件
        P->>P: 更新状态

        alt 达到检查点间隔
            P->>C: 保存检查点<br/>offset=10000
            Note over P: 故障后可从检查点恢复
        end
    end

    style C fill:#9ff,stroke:#333
```

## 六、实践最佳实践

### 6.1 事件设计指南

```mermaid
graph TD
    A[事件设计] --> B[事件命名]
    A --> C[事件内容]
    A --> D[事件版本]
    A --> E[事件签名]

    B --> B1[过去时态]
    B --> B2[领域驱动]

    C --> C1[最小够用]
    C --> C2[包含足够上下文]

    D --> D1[版本号]
    D --> D2[兼容策略]

    style A fill:#ff9,stroke:#333
```

### 6.2 性能优化

```mermaid
graph TD
    A[性能优化] --> B[快照优化]
    A --> C[并行重放]
    A --> D[增量投影]
    A --> E[缓存策略]

    B --> B1[合理快照间隔]
    B --> B2[快照压缩]

    C --> C1[事件分片]
    C --> C2[并行处理]

    D --> D1[只更新变化部分]
    D --> D2[避免全量重建]

    style A fill:#ff9,stroke:#333
```

### 6.3 一致性保证

```mermaid
graph TD
    A[一致性保证] --> B[聚合内]
    A --> C[聚合间]
    A --> D[跨系统]

    B --> B1[强一致性]
    B --> B2[事务保证]

    C --> C1[最终一致性]
    C --> C2[事件驱动]

    D --> D1[Saga模式]
    D --> D2[补偿事务]

    style A fill:#ff9,stroke:#333
```

## 七、总结与启示

核心要点：
- 有状态服务需要仔细设计状态管理和持久化策略
- 事件溯源提供完整的状态变更历史，支持时间旅行
- 聚合根模式定义了一致性边界
- 快照机制优化状态恢复性能

---

*本章精读笔记完成*
