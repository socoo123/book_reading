# 第五章：Request/Response

## 一、请求响应模式概述

### 1.1 请求响应定义

请求-响应（Request/Response）是分布式系统中最基本的通信模式：

```mermaid
sequenceDiagram
    participant C as 客户端
    participant S as 服务端

    C->>S: 请求 (Request)
    S->>S: 处理请求
    S-->>C: 响应 (Response)

    Note over C,S: 同步等待响应返回
```

**核心特征**：
- **同步性**：客户端等待服务端响应
- **一对一**：一个请求对应一个响应
- **双向通信**：请求和响应都携带数据

### 1.2 请求响应模式分类

```mermaid
graph TD
    A[请求响应模式] --> B[同步模式]
    A --> C[异步模式]
    A --> D[流式模式]

    B --> B1[阻塞等待]
    B --> B2[简单直观]

    C --> C1[非阻塞]
    C --> C2[回调处理]

    D --> D1[持续数据流]
    D --> D2[分块传输]

    style A fill:#ff9,stroke:#333
```

## 二、同步请求响应

### 2.1 同步模式架构

同步请求响应的基本架构：

```mermaid
graph TD
    subgraph 同步请求响应
    Client[客户端] --> LB[负载均衡器]
    LB --> Server1[服务节点1]
    LB --> Server2[服务节点2]
    LB --> Server3[服务节点3]

    Server1 --> DB[(数据库)]
    Server2 --> DB
    Server3 --> DB
    end

    style Client fill:#9ff,stroke:#333
    style LB fill:#f96,stroke:#333
    style Server1 fill:#9f9,stroke:#333
```

### 2.2 同步调用流程

```mermaid
sequenceDiagram
    participant C as 客户端
    participant LB as 负载均衡器
    participant S as 服务端
    participant DB as 数据库

    C->>LB: HTTP GET /api/resource
    LB->>LB: 选择服务节点
    LB->>S: 转发请求
    S->>DB: 查询数据
    DB-->>S: 返回结果
    S-->>LB: HTTP 200 + JSON
    LB-->>C: 返回响应

    Note over C,S: 整个过程同步阻塞
```

### 2.3 同步模式优缺点

| 优点 | 缺点 |
|-----|------|
| 简单直观，易于理解和调试 | 客户端阻塞等待，降低用户体验 |
| 请求-响应语义清晰 | 长连接消耗资源 |
| 适合短时间操作 | 服务器故障会立即暴露 |
| 易于实现重试逻辑 | 无法支持推送场景 |

## 三、异步请求响应

### 3.1 异步模式架构

异步请求响应通过回调或轮询处理响应：

```mermaid
graph TD
    subgraph 异步请求响应
    Client[客户端] --> Broker[消息中间件]
    Broker --> Server[服务节点]
    Server --> Broker[结果队列]
    Broker --> Client[回调通知]

    Client --> C1[发起请求<br/>非阻塞]
    Client --> C2[继续处理其他任务]
    C2 --> C3[接收回调]
    end

    style Client fill:#9ff,stroke:#333
    style Broker fill:#f96,stroke:#333
```

### 3.2 异步调用流程

```mermaid
sequenceDiagram
    participant C as 客户端
    participant Q as 请求队列
    participant S as 服务端
    participant R as 结果队列
    participant C2 as 回调处理

    C->>Q: 发送异步请求
    Q-->>C: 确认收到 (correlationId)
    Note over C: 继续处理其他任务

    Q->>S: 获取请求
    S->>S: 处理业务
    S->>R: 发送结果 (correlationId)

    R->>C2: 推送回调
    C2->>C: 处理响应结果

    Note over C,S: 全程非阻塞
```

### 3.3 异步实现方式

```mermaid
graph TD
    A[异步实现方式] --> B[回调函数]
    A --> C[Promise/Future]
    A --> D[事件监听]
    A --> E[消息队列]

    B --> B1[回调地狱风险]
    B --> B2[适合简单场景]

    C --> C1[链式调用]
    C --> C2[现代语言支持]

    D --> D1[事件驱动]
    D --> D2[解耦程度高]

    E --> E1[可靠性高]
    E --> E2[支持重试]

    style A fill:#ff9,stroke:#333
```

### 3.4 Future模式实现

```mermaid
graph TD
    subgraph Future模式
    Client[客户端] --> Future[Future对象]
    Future --> S[提交任务]
    S --> ThreadPool[线程池]

    ThreadPool --> Task[异步执行]
    Task --> Result[计算结果]

    Client --> C1[立即返回]
    C1 --> C2[轮询结果]
    C2 --> Future

    Result --> Future
    Future --> C3[获取结果]
    end

    style Future fill:#9ff,stroke:#333
```

## 四、消息队列异步通信

### 4.1 消息队列架构

使用消息队列实现异步通信：

```mermaid
graph TD
    subgraph 消息队列架构
    Producer[生产者] --> Q[消息队列]
    Q --> Consumer1[消费者1]
    Q --> Consumer2[消费者2]
    Q --> Consumer3[消费者3]

    Producer --> P1[发送消息]
    Consumer1 --> C1[处理消息]
    Consumer2 --> C2[处理消息]
    Consumer3 --> C3[处理消息]
    end

    style Q fill:#f96,stroke:#333
    style Producer fill:#9ff,stroke:#333
    style Consumer1 fill:#9f9,stroke:#333
```

### 4.2 消息队列核心概念

```mermaid
graph TD
    A[消息队列核心概念] --> B[队列]
    A --> C[生产者]
    A --> D[消费者]
    A --> E[消息]
    A --> F[订阅]

    B --> B1[先进先出]
    B --> B2[持久化存储]

    C --> C1[发送消息]
    C --> C2[不知道消费者]

    D --> D1[接收消息]
    D --> D2[不知道生产者]

    style A fill:#ff9,stroke:#333
```

### 4.3 消息模式

```mermaid
graph TD
    A[消息模式] --> B[点对点]
    A --> C[发布订阅]

    subgraph 点对点
    P1[生产者1] --> Q[队列]
    P2[生产者2] --> Q
    Q --> C1[消费者1]
    Q --> C2[消费者2]
    end

    subgraph 发布订阅
    P[生产者] --> Topic[主题]
    Topic --> S1[订阅者1]
    Topic --> S2[订阅者2]
    Topic --> S3[订阅者3]
    end

    style B fill:#9ff,stroke:#333
    style C fill:#9f9,stroke:#333
```

### 4.4 消息队列对比

| 特性 | RabbitMQ | Kafka | RocketMQ |
|-----|----------|-------|----------|
| **协议** | AMQP | 自定义 | 自定义 |
| **持久化** | 支持 | 高性能 | 支持 |
| **顺序消息** | 支持 | 支持 | 支持 |
| **吞吐量** | 中 | 高 | 高 |
| **适用场景** | 企业集成 | 日志/流处理 | 电商订单 |

## 五、负载均衡

### 5.1 负载均衡策略

```mermaid
graph TD
    A[负载均衡策略] --> B[轮询]
    A --> C[加权轮询]
    A --> D[最少连接]
    A --> E[一致性哈希]
    A --> F[随机]

    B --> B1[按顺序分配]
    B --> B2[简单公平]

    C --> C1[按权重分配]
    C --> C2[处理异构节点]

    D --> D1[连接数最少]
    D --> D2[动态负载]

    E --> E1[相同Key路由相同节点]
    E --> E2[缓存友好]

    style A fill:#ff9,stroke:#333
```

### 5.2 负载均衡架构

```mermaid
graph TD
    subgraph 负载均衡架构
    Client[客户端] --> L4[4层负载均衡<br/>TCP/IP]
    Client --> L7[7层负载均衡<br/>HTTP/DNS]

    L4 --> S1[服务器1]
    L4 --> S2[服务器2]
    L4 --> S3[服务器3]

    L7 --> S1
    L7 --> S2
    L7 --> S3

    L4 --> LB1[LVS/Nginx]
    L7 --> LB2[Nginx/HAProxy]
    end

    style Client fill:#9ff,stroke:#333
```

### 5.3 健康检查

```mermaid
graph TD
    A[健康检查] --> B[主动检查]
    A --> C[被动检查]
    A --> D[检查方式]

    B --> B1[定期探测]
    B --> B2[HTTP/TCP探测]

    C --> C1[基于请求]
    C --> C2[基于响应]

    D --> D1[心跳机制]
    D --> D2[探测端口]
    D --> D3[检查接口]

    style A fill:#ff9,stroke:#333
```

## 六、故障处理

### 6.1 重试机制

```mermaid
graph TD
    A[重试策略] --> B[立即重试]
    A --> C[指数退避]
    A --> D[固定间隔]
    A --> E[熔断器]

    B --> B1[适合瞬时故障]
    B --> B2[可能加重拥塞]

    C --> C1[间隔逐渐增加]
    C --> C2[避免风暴]

    D --> D1[固定等待时间]
    D --> D2[简单可预测]

    E --> E1[快速失败]
    E --> E2[防止级联故障]

    style A fill:#ff9,stroke:#333
```

### 6.2 重试流程

```mermaid
sequenceDiagram
    participant C as 客户端
    participant S as 服务端

    C->>S: 请求1
    S-->>C: 失败 (5xx)
    Note over C: 等待退避时间

    C->>S: 请求2
    S-->>C: 失败 (5xx)
    Note over C: 再次等待

    C->>S: 请求3
    S-->>C: 成功 (200)
    Note over C: 重试成功

    Note over C: 超过最大重试次数后放弃
```

### 6.3 超时设置

```mermaid
graph TD
    A[超时设置原则] --> B[分级超时]
    A --> C[合理时间]
    A --> D[可配置]
    A --> E[监控告警]

    B --> B1[前端短]
    B --> B2[后端长]

    C --> C1[考虑P99延迟]
    C --> C2[考虑业务场景]

    style A fill:#ff9,stroke:#333
```

### 6.4 降级策略

```mermaid
graph TD
    A[降级策略] --> B[返回缓存]
    A --> C[返回默认值]
    A --> D[限流拒绝]
    A --> E[功能降级]

    B --> B1[使用旧数据]
    B --> B2[牺牲一致性]

    C --> C1[返回默认响应]
    C --> C2[保证可用性]

    style A fill:#f96,stroke:#333
```

## 七、总结与启示

核心要点：
- 同步请求响应简单直观，但会阻塞客户端
- 异步请求响应提高系统吞吐量和用户体验
- 消息队列实现生产者和消费者的完全解耦
- 负载均衡和故障处理是保证系统可靠性的关键

---

*本章精读笔记完成*
