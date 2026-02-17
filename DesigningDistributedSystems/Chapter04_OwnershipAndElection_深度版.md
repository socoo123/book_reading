# 第四章：Ownership and Election

## 一、领导选举概述

### 1.1 领导选举定义

领导选举（Leader Election）是在分布式系统中选择协调者的过程：

```mermaid
graph TD
    A[领导选举] --> B[多个候选者]
    A --> C[公平竞争]
    A --> D[唯一胜出]
    A --> E[故障切换]

    subgraph 选举过程
    N1[节点1] --> V1[投票]
    N2[节点2] --> V2[投票]
    N3[节点3] --> V3[投票]
    V1 --> W[胜出者]
    V2 --> W
    V3 --> W
    end

    style A fill:#ff9,stroke:#333
```

**核心目标**：从多个节点中选择一个作为主协调者，确保系统有且只有一个领导者。

### 1.2 领导选举的必要性

什么时候需要领导选举：

```mermaid
graph TD
    A[需要领导选举的场景] --> B[单点协调]
    A --> C[数据一致性]
    A --> D[任务分配]
    A --> E[状态同步]

    B --> B1[需要唯一的协调者]
    B --> B2[如：主节点选举]

    C --> C1[避免并发冲突]
    C --> C2[如：分布式锁]

    style A fill:#ff9,stroke:#333
```

## 二、领导选举算法

### 2.1 Bully算法

Bully算法是最简单的领导选举算法：

```mermaid
graph TD
    A[Bully算法] --> B[节点ID比较]
    A --> C[ID最大者胜出]
    A --> D[故障检测触发]

    subgraph 选举流程
    Step1[节点发现主节点故障] --> Step2[发起选举]
    Step2 --> Step3[向ID更高的节点发送选举消息]
    Step3 --> Step4[更高ID节点响应并接管选举]
    Step4 --> Step5[最高ID节点成为新主]
    Step5 --> Step6[广播新主信息]
    end

    style A fill:#ff9,stroke:#333
```

**Bully算法特点**：
- **简单**：实现简单，易于理解
- **快速**：收敛时间短
- **缺点**：ID最大的节点可能成为性能瓶颈

### 2.2 Ring算法

环算法将节点组织成环状结构：

```mermaid
graph LR
    subgraph 环结构
    N1[节点1] --> N2[节点2]
    N2 --> N3[节点3]
    N3 --> N4[节点4]
    N4 --> N5[节点5]
    N5 --> N6[节点6]
    N6 --> N1
    end

    style N1 fill:#9ff,stroke:#333
    style N3 fill:#f96,stroke:#333
    style N6 fill:#9f9,stroke:#333
```

**环算法流程**：
1. 节点按ID排序形成环
2. 检测到故障的节点发起选举
3. 沿环传递选举消息
4. 包含最大ID的消息回到发起者
5. 发起者宣布新领导

### 2.3 Raft算法

Raft是一种现代的共识算法：

```mermaid
graph TD
    A[Raft算法] --> B[Leader]
    A --> C[Candidate]
    A --> D[Follower]

    B --> B1[处理客户端请求]
    B --> B2[复制日志到从节点]
    B --> B3[维持心跳]

    C --> C1[候选人]
    C --> C2[请求投票]
    C --> C3[成为Leader]

    D --> D1[响应Leader]
    D --> D2[响应Candidate]
    D --> D3[超时后变为Candidate]

    style A fill:#ff9,stroke:#333
    style B fill:#9ff,stroke:#333
    style C fill:#f96,stroke:#333
    style D fill:#9f9,stroke:#333
```

**Raft核心机制**：
```mermaid
sequenceDiagram
    participant L as Leader
    participant F1 as Follower1
    participant F2 as Follower2
    participant C as Client

    C->>L: 请求
    L->>L: 追加日志
    L->>F1: 复制日志(AppendEntries)
    L->>F2: 复制日志(AppendEntries)
    F1-->>L: 确认
    F2-->>L: 确认
    L->>L: 提交日志
    L->>F1: 提交确认
    L->>F2: 提交确认
    L-->>C: 响应

    Note over L,F1: 心跳定期发送<br/>维持Leader地位
```

### 2.4 算法对比

| 算法 | 复杂度 | 容错能力 | 实现难度 | 适用场景 |
|-----|-------|---------|---------|---------|
| **Bully** | O(n) | f < n/2 | 低 | 小规模集群 |
| **Ring** | O(n) | f < n/2 | 低 | 环状网络 |
| **Raft** | O(n) | f < n/3 | 中 | 生产环境 |
| **Paxos** | O(n) | f < n/2 | 高 | 学术/复杂场景 |

## 三、分布式锁

### 3.1 分布式锁概念

分布式锁是跨节点的互斥机制：

```mermaid
graph TD
    A[分布式锁] --> B[互斥访问]
    A --> C[超时机制]
    A --> D[可重入性]
    A --> E[公平性]

    subgraph 锁操作
    N1[节点1] --> L[获取锁]
    L --> N1[持有锁]
    N1 --> R[释放锁]
    R --> N2[节点2获取锁]
    end

    style A fill:#ff9,stroke:#333
```

### 3.2 基于ZooKeeper的分布式锁

使用ZooKeeper实现分布式锁：

```mermaid
graph TD
    subgraph ZK锁实现
    Client1[客户端1] --> ZK[ZooKeeper]
    Client2[客户端2] --> ZK

    ZK --> LockNode[/locks/my_lock]
    LockNode --> Ephemeral1[临时节点1<br/>客户端1]
    LockNode --> Ephemeral2[临时节点2<br/>客户端2]
    LockNode --> Ephemeral3[临时节点3<br/>客户端3]

    Ephemeral1 --> Watcher1[Watcher监听]
    Ephemeral2 --> Watcher2
    Ephemeral3 --> Watcher3
    end

    style LockNode fill:#9ff,stroke:#333
```

**ZK锁获取流程**：
1. 在/locks下创建临时顺序节点
2. 获取所有子节点，判断是否最小
3. 如果是最小节点，获取锁成功
4. 否则监听前一个节点的删除事件
5. 收到通知后重新检查

### 3.3 基于etcd的分布式锁

使用etcd的lease和watch机制：

```mermaid
sequenceDiagram
    participant C1 as 客户端1
    participant E as etcd
    participant C2 as 客户端2

    C1->>E: Put key=lock, value=C1, lease=30s
    E-->>C1: OK
    C1->>E: Watch key=lock
    E-->>C1: 持续监控

    Note over C1: 持有锁，可以访问资源

    C2->>E: Put key=lock, value=C2
    E-->>C2: 失败（已存在）

    C1->>E: Delete key=lock
    E-->>C1: OK
    E-->>C2: Watch事件（锁可用）
    C2->>E: Put key=lock, value=C2, lease=30s
    E-->>C2: OK

    Note over C2: 获取到锁
```

### 3.4 分布式锁使用场景

```mermaid
graph TD
    A[分布式锁应用] --> B[资源互斥]
    A --> C[任务调度]
    A --> D[限流控制]
    A --> E[配置更新]

    B --> B1[防止并发修改]
    B --> B2[如：库存扣减]

    C --> C1[防止重复执行]
    C --> C2[如：定时任务]

    D --> D1[限制并发数量]
    D --> D2[如：秒杀]

    style A fill:#ff9,stroke:#333
```

## 四、碎片的动态所有权

### 4.1 动态所有权概念

动态所有权（Dynamic Ownership）允许分片在不同节点间迁移：

```mermaid
graph TD
    A[动态所有权] --> B[按需分配]
    A --> C[负载均衡]
    A --> D[故障转移]
    A --> E[弹性扩展]

    subgraph 所有权迁移
    Before[迁移前<br/>分片A在节点1] --> After[迁移后<br/>分片A在节点2]
    end

    style A fill:#ff9,stroke:#333
```

### 4.2 所有权迁移流程

```mermaid
sequenceDiagram
    participant M as 协调者
    participant S1 as 源节点
    participant S2 as 目标节点
    participant C as 客户端

    M->>S1: 触发迁移
    S1->>S2: 开始数据同步
    loop 同步数据
        S1->>S2: 传输分片数据
        S2->>S2: 写入数据
    end
    S2->>M: 同步完成
    M->>M: 更新路由表
    M->>C: 更新客户端路由
    C->>S2: 新请求
    S1->>M: 确认客户端切换
    M->>S1: 停止服务，清理数据

    Note over M,S2: 迁移过程保持服务可用
```

### 4.3 所有权管理策略

```mermaid
graph TD
    A[所有权策略] --> B[集中式管理]
    A --> C[分布式协商]
    A --> D[基于租约]

    B --> B1[主节点决定]
    B --> B2[简单但单点]

    C --> C1[节点协商]
    C --> C2[复杂但无单点]

    D --> D1[租约控制]
    D --> D2[平衡两者]

    style A fill:#ff9,stroke:#333
```

## 五、实现最佳实践

### 5.1 选举算法选择

```mermaid
graph TD
    A[选择选举算法] --> B{集群规模?}
    B -->|< 10节点| C[Bully算法]
    B -->|> 10节点| D{一致性要求?}
    D -->|强一致性| E[Raft/Paxos]
    D -->|最终一致性| F[Gossip协议]

    C --> G[实现简单]
    E --> H[生产环境推荐]
    F --> I[大规模集群]

    style A fill:#ff9,stroke:#333
```

### 5.2 故障检测

```mermaid
graph TD
    A[故障检测] --> B[心跳机制]
    A --> C[超时检测]
    A --> D[租约管理]

    B --> B1[定期发送心跳]
    B --> B2[超时判定死亡]

    C --> C3[设置合理超时]
    C --> C4[考虑网络抖动]

    D --> D1[租约有效期]
    D --> D2[续约机制]

    style A fill:#ff9,stroke:#333
```

### 5.3 安全性考虑

```mermaid
graph TD
    A[安全考虑] --> B[身份认证]
    A --> C[权限控制]
    A --> D[通信加密]
    A --> E[防脑裂]

    B --> B1[节点身份验证]
    B --> B2[防止冒充]

    C --> C1[限制操作权限]
    C --> C2[最小权限原则]

    D --> D1[TLS加密通信]
    D --> D2[防止窃听]

    E --> E1[多数派机制]
    E --> E2[避免双主]

    style A fill:#f96,stroke:#333
```

## 六、总结与启示

核心要点：
- 领导选举确保分布式系统中有唯一的协调者
- Bully、Ring算法适用于简单场景，Raft适用于生产环境
- 分布式锁实现资源互斥，支持多种应用场景
- 动态所有权支持分片迁移，实现负载均衡和故障转移

---

*本章精读笔记完成*
