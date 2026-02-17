# 第三章：Sharded Services

## 一、分片架构概述

### 1.1 分片定义

分片（Sharding）是将数据分散存储在多个节点上的技术：

```mermaid
graph TD
    A[分片架构] --> B[数据分区]
    A --> C[多节点存储]
    A --> D[独立扩展]
    A --> E[负载分散]

    subgraph 数据分布
    Data[原始数据] --> Partition1[分片1]
    Data --> Partition2[分片2]
    Data --> Partition3[分片3]
    Data --> Partition4[分片4]
    end

    style A fill:#ff9,stroke:#333
```

**核心思想**：将大型数据集分割成较小的、可管理的部分，每个分片存储在不同的服务器上。

### 1.2 分片 vs 复制

分片与复制是两种不同的数据分布策略：

```mermaid
graph TD
    subgraph 分片[Sharding]
    A1[数据A] --> A2[分片1]
    A1 --> A3[分片2]
    B1[数据B] --> A4[分片1]
    B1 --> A5[分片2]
    end

    subgraph 复制[Replication]
    C1[数据A] --> D1[节点1]
    C1 --> D2[节点2]
    C1 --> D3[节点3]
    end

    style A1 fill:#9ff,stroke:#333
    style C1 fill:#f96,stroke:#333
```

| 特性 | 分片 | 复制 |
|-----|------|-----|
| **目的** | 水平扩展存储能力 | 提高可用性和读取性能 |
| **数据分布** | 不同节点存储不同数据 | 所有节点存储相同数据 |
| **写入扩展** | 可以线性扩展写入 | 写入无法扩展 |
| **一致性** | 需要跨分片协调 | 副本同步 |

## 二、分片模式

### 2.1 水平分片

水平分片按行分割数据：

```mermaid
graph TD
    subgraph 原始表
    R1[id:1, name:A, region:East]
    R2[id:2, name:B, region:West]
    R3[id:3, name:C, region:East]
    R4[id:4, name:D, region:West]
    end

    subgraph 水平分片
    P1[分片1：East区域<br/>R1, R3]
    P2[分片2：West区域<br/>R2, R4]
    end

    R1 --> P1
    R3 --> P1
    R2 --> P2
    R4 --> P2

    style P1 fill:#9ff,stroke:#333
    style P2 fill:#9f9,stroke:#333
```

**水平分片特点**：
- 每个分片包含部分行
- 所有分片有相同的列结构
- 按行分割数据

### 2.2 垂直分片

垂直分片按列分割数据：

```mermaid
graph TD
    subgraph 原始表
    T[id, name, email, phone, address, profile, orders, preferences]
    end

    subgraph 垂直分片
    V1[分片1：用户信息<br/>id, name, email, phone]
    V2[分片2：地址信息<br/>id, address]
    V3[分片3：行为信息<br/>id, profile, orders, preferences]
    end

    style V1 fill:#9ff,stroke:#333
    style V2 fill:#9f9,stroke:#333
    style V3 fill:#f99,stroke:#333
```

**垂直分片特点**：
- 每个分片包含部分列
- 每个分片可以独立优化
- 适合列访问模式不同的情况

### 2.3 混合分片

结合水平和垂直分片：

```mermaid
graph TD
    A[混合分片] --> B[先水平分片]
    B --> C[每个分片内再垂直分片]

    subgraph 混合架构
    Data[用户数据] --> P1[分片1]
    Data --> P2[分片2]

    P1 --> V1_1[分片1-1<br/>基础信息]
    P1 --> V1_2[分片1-2<br/>详细信息]

    P2 --> V2_1[分片2-1<br/>基础信息]
    P2 --> V2_2[分片2-2<br/>详细信息]
    end

    style A fill:#ff9,stroke:#333
```

## 三、分片键选择

### 3.1 分片键的重要性

分片键是决定数据分配策略的关键字段：

```mermaid
graph TD
    A[分片键选择] --> B[查询性能]
    A --> C[数据分布]
    A --> D[扩展能力]
    A --> E[一致性保证]

    B --> B1[直接影响查询路由]
    B --> B2[影响join性能]

    C --> C1[避免数据倾斜]
    C --> C2[均匀分布负载]

    style A fill:#ff9,stroke:#333
```

### 3.2 分片键类型

常见的分片键类型：

```mermaid
graph TD
    A[分片键类型] --> B[哈希分片]
    A --> C[范围分片]
    A --> D[目录分片]
    A --> E[地理分片]

    B --> B1[均匀分布]
    B --> B2[扩展困难]

    C --> C1[范围查询高效]
    C --> C2[可能热点]

    D --> D1[灵活映射]
    D --> D2[维护成本高]

    E --> E1[本地化访问]
    E --> E2[合规需求]

    style A fill:#ff9,stroke:#333
```

### 3.3 分片策略对比

| 策略 | 优点 | 缺点 | 适用场景 |
|-----|------|------|---------|
| **哈希分片** | 数据均匀 | 范围查询低效 | 随机访问 |
| **范围分片** | 范围查询快 | 可能热点 | 时间序列 |
| **目录分片** | 灵活 | 维护复杂 | 多租户 |
| **地理分片** | 低延迟 | 跨区域查询复杂 | 全球化应用 |

### 3.4 分片键选择最佳实践

```mermaid
flowchart TD
    A[选择分片键] --> B{主要查询模式?}
    B -->|点查询为主| C[使用哈希分片]
    B -->|范围查询为主| D[使用范围分片]
    B -->|多维度查询| E[使用复合分片键]

    C --> F{数据均匀?}
    F -->|是| G[适合使用]
    F -->|否| H[考虑其他策略]

    D --> I{热点问题?}
    I -->|是| J[添加热点分片]
    I -->|否| K[适合使用]

    style A fill:#ff9,stroke:#333
```

## 四、分片再平衡

### 4.1 何时需要再平衡

触发分片再平衡的场景：

```mermaid
graph TD
    A[触发再平衡] --> B[节点添加/删除]
    A --> C[数据分布不均]
    A --> D[负载过高]
    A --> E[存储空间不足]

    B --> B1[集群扩容]
    B --> B2[节点故障]

    C --> C1[数据倾斜]
    C --> C2[访问倾斜]

    style A fill:#ff9,stroke:#333
    style B fill:#f96,stroke:#333
```

### 4.2 再平衡策略

```mermaid
graph TD
    A[再平衡策略] --> B[手动再平衡]
    A --> C[自动再平衡]
    A --> D[虚拟分片]

    B --> B1[管理员触发]
    B --> B2[可控但低效]

    C --> C1[阈值触发]
    C --> C2[动态调整]

    D --> D1[增加分片数量]
    D --> D2[数据移动量小]

    style A fill:#ff9,stroke:#333
```

### 4.3 再平衡过程

```mermaid
sequenceDiagram
    participant A as 管理员/系统
    participant M as 主节点
    participant S1 as 旧分片
    participant S2 as 新分片

    A->>M: 触发再平衡
    M->>M: 计算新分片映射
    M->>S1: 开始数据迁移
    loop 迁移数据
        S1->>S2: 传输数据
        S2->>S2: 写入数据
    end
    S1->>M: 迁移完成
    M->>M: 更新路由表
    M->>S1: 标记只读
    M->>M: 切换流量
    M->>S1: 清理数据

    Note over M,S2: 过程中保证服务可用
```

### 4.4 数据迁移策略

| 策略 | 描述 | 优点 | 缺点 |
|-----|------|------|------|
| **离线迁移** | 停止服务后迁移 | 简单 | 停机时间长 |
| **在线不中断迁移迁移** | 服务 | 用户无感知 | 复杂 |
| **双写策略** | 同时写入新旧节点 | 平滑过渡 | 写入延迟 |

## 五、分片与复制结合

### 5.1 分片+复制架构

分片和复制通常结合使用：

```mermaid
graph TD
    subgraph 分片+复制架构
    Client[客户端] --> Router[路由层]

    Router --> P1[分片1]
    Router --> P2[分片2]
    Router --> P3[分片3]

    subgraph 副本组1
    P1 --> R1_1[主节点]
    P1 --> R1_2[从节点1]
    P1 --> R1_3[从节点2]
    end

    subgraph 副本组2
    P2 --> R2_1[主节点]
    P2 --> R2_2[从节点]
    end

    subgraph 副本组3
    P3 --> R3_1[主节点]
    P3 --> R3_2[从节点]
    end
    end

    style Router fill:#9ff,stroke:#333
```

### 5.2 写入流程

结合复制后的写入流程：

```mermaid
sequenceDiagram
    participant C as 客户端
    participant R as 路由层
    participant M as 主节点
    participant S as 从节点

    C->>R: 写入请求
    R->>R: 确定目标分片
    R->>M: 转发到主节点
    M->>M: 写入本地
    M->>S: 同步到从节点
    S-->>M: 确认
    M-->>R: 确认
    R-->>C: 写入成功

    Note over M,S: 可配置同步/异步复制
```

### 5.3 读请求优化

读请求可以根据一致性要求路由到不同节点：

```mermaid
graph TD
    A[读请求路由] --> B[强一致性]
    A --> C[最终一致性]
    A --> D[本地读取]

    B --> B1[必须读主节点]
    B --> B2[返回最新数据]

    C --> C1[可读从节点]
    C --> C2[可能返回旧数据]

    D --> D1[读本地副本]
    D --> D2[最低延迟]

    style A fill:#ff9,stroke:#333
```

## 六、总结与启示

核心要点：
- 分片通过数据分区实现水平扩展
- 分片键选择直接影响系统性能和扩展能力
- 分片再平衡是保持系统均衡的关键机制
- 分片与复制结合可以同时获得扩展性和高可用性

---

*本章精读笔记完成*
