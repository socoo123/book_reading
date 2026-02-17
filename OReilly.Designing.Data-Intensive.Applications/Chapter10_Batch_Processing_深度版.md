# 《Designing Data-Intensive Applications》- 第10章：批处理（深度版）

## 一、本章概述

本章深入探讨了**批处理（Batch Processing）**技术，这是大规模数据处理的基础范式之一。批处理系统通过对大量静态数据进行批量计算，提供了强大的数据处理能力和高吞吐量。

> **本章核心问题**：如何高效地处理 TB/PB 级别的静态数据？批处理系统如何实现容错和水平扩展？

### 1.1 核心主题
- 批处理的本质与特点
- MapReduce 模型详解
- 数据流引擎与 Spark
- 批处理优化技术
- 批处理的应用场景

### 1.2 重要程度
⭐⭐⭐⭐（高）

### 1.3 预计学习时间
100-120 分钟

### 1.4 本章与其他章节的关联

```mermaid
graph TD
    A[第10章: 批处理] --> B[第11章: 流处理]
    A --> C[第3章: 存储引擎]
    A --> D[第6章: 分区]

    B -->|Lambda架构| A
    C -->|数据存储| A
    D -->|数据分区| A

    style A fill:#ff9,stroke:#333
```

---

## 二、批处理的本质

### 2.1 批处理的特点

```mermaid
graph TD
    subgraph 批处理特征["批处理特征"]
        B1[输入: 有限数据集]
        B2[输出: 完整结果]
        B3[时间: 几分钟到几小时]
        B4[模式: 一次性处理]
        B5[容错: 任务重试]
    end

    subgraph 与流处理对比["批处理 vs 流处理"]
        C1["批处理: 处理历史数据"]
        C2["流处理: 处理实时数据"]
        C1 -->|互补| C2
    end
```

**批处理的核心特点：**

| 特点 | 说明 | 优势 |
|-----|-----|-----|
| **数据有限** | 处理固定大小的数据集 | 可预测的资源使用 |
| **一次性** | 任务执行后结束 | 简化容错处理 |
| **高吞吐** | 优化批量读写 | 处理大规模数据 |
| **容错性** | 失败后可重跑 | 结果准确 |

### 2.2 批处理的应用场景

```mermaid
graph TD
    subgraph 批处理场景["批处理典型场景"]
        A1[ETL 数据处理]
        A2[数据分析报表]
        A3[机器学习训练]
        A4[日志聚合分析]
        A5[数据仓库填充]
        A6[全量索引构建]
    end

    A1 --> A11["数据清洗"]
    A1 --> A12["格式转换"]
    A2 --> A21["日/周/月报"]
    A3 --> A31["模型训练"]
    A3 --> A32["特征工程"]
```

---

## 三、MapReduce 模型

### 3.1 MapReduce 概述

**MapReduce** 是 Google 提出的经典批处理编程模型，将计算分为 Map 和 Reduce 两个阶段。

```mermaid
graph TD
    subgraph MapReduce流程["MapReduce 执行流程"]
        I1[Input] --> M1[Map]
        M1 --> S[Shuffle & Sort]
        S --> R[Reduce]
        R --> O[Output]

        M1 --> M2[Map]
        M2 --> S
        S --> R2[Reduce]
    end

    subgraph 分布式["分布式执行"]
        D1[任务调度]
        D2[数据分区]
        D3[容错重试]
        D4[结果合并]
    end
```

### 3.2 Map 阶段详解

**Map 函数**：将输入数据转换为键值对。

```mermaid
graph TD
    subgraph Map处理["Map 处理过程"]
        Input["日志行"] --> P[解析]
        P --> K[提取 key]
        P --> V[生成 value]

        K --> Output["(key, value) 对"]
    end

    subgraph Map示例["Map 示例: 词频统计"]
        S1["The quick brown fox"]
        S2["The lazy dog"]

        M1["(The, 1), (quick, 1), (brown, 1), (fox, 1)"]
        M2["(The, 1), (lazy, 1), (brown, 1), (dog, 1)"]
    end
```

### 3.3 Shuffle 阶段详解

**Shuffle**：Map 和 Reduce 之间的数据分发过程。

```mermaid
graph TD
    subgraph Shuffle过程["Shuffle 过程"]
        M1[Map Task 1] --> P1[分区]
        M2[Map Task 2] --> P2[分区]

        P1 --> S1[排序]
        P2 --> S2[排序]

        S1 --> C1[Combiner 可选]
        S2 --> C2[Combiner 可选]

        C1 --> R1[Reduce Task 1]
        C2 --> R2[Reduce Task 2]
    end

    subgraph 数据流["数据流"]
        K1["key=a 的数据"] --> R1
        K2["key=b 的数据"] --> R2
    end
```

**Shuffle 的关键步骤：**

```mermaid
flowchart TD
    A[Map 输出] --> B[分区 Partition]
    B --> C[排序 Sort]
    C --> D[溢写 Spill]
    D --> E[合并 Merge]

    B --> F["按 Key 哈希决定 Reduce"]
    C --> G["按 Key 排序"]
    D --> H["写入磁盘"]
    E --> I["多个文件合并"]
```

### 3.4 Reduce 阶段详解

**Reduce 函数**：对相同 key 的所有 value 进行聚合。

```mermaid
graph TD
    subgraph Reduce处理["Reduce 处理过程"]
        Input["(key, [value1, value2, ...])"] --> A[聚合]
        A --> O["(key, result)"]
    end

    subgraph Reduce示例["Reduce 示例: 词频统计"]
        I1["(The, [1, 1])"] --> R[SUM]
        O1["(The, 2)"]

        I2["(quick, [1])"] --> R2
        O2["(quick, 1)"]
    end
```

### 3.5 MapReduce 代码示例

**WordCount 示例：**

```java
// Mapper
public static class WordCountMapper
    extends Mapper<LongWritable, Text, Text, IntWritable> {

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(LongWritable key, Text value, Context context) {
        String line = value.toString();
        String[] words = line.split(" ");

        for (String w : words) {
            word.set(w);
            context.write(word, one);
        }
    }
}

// Reducer
public static class WordCountReducer
    extends Reducer<Text, IntWritable, Text, IntWritable> {

    public void reduce(Text key, Iterable<IntWritable> values, Context context) {
        int sum = 0;
        for (IntWritable v : values) {
            sum += v.get();
        }
        context.write(key, new IntWritable(sum));
    }
}
```

### 3.6 MapReduce 的容错机制

```mermaid
graph TD
    subgraph 容错机制["MapReduce 容错机制"]
        T1[任务级失败]
        T2[任务尝试失败]
        T3[节点失败]
        T4[推测执行]

        T1 --> T1a["重试任务"]
        T2 --> T2a["尝试其他节点"]
        T3 --> T3a["重新调度任务]
        T4 --> T4a["同时执行多个副本]
    end
```

**容错策略：**

| 失败类型 | 处理方式 | 重试次数 |
|---------|---------|---------|
| Map 任务失败 | 在其他节点重新执行 | 通常 4 次 |
| Reduce 任务失败 | 重新获取 Map 输出并执行 | 通常 4 次 |
| 节点故障 | 调度器重新分配任务 | 自动检测 |
| 任务卡住 | 启动备份任务 | 推测执行 |

---

## 四、数据流引擎

### 4.1 MapReduce 的局限性

```mermaid
graph TD
    subgraph MapReduce问题["MapReduce 的问题"]
        P1[迭代效率低]
        P2[中间结果写磁盘]
        P3[表达能力有限]
        P4[资源利用低]

        P1 -->|"每次迭代都要重跑"| I[迭代计算慢]
        P2 -->|"Map → Shuffle → Reduce"| D[频繁 IO]
    end
```

| 问题 | 影响 | 改进方向 |
|-----|-----|---------|
| **迭代计算** | 无法高效支持 ML、图计算 | 引入 DAG 执行 |
| **中间结果** | 每次都写 HDFS | 内存缓存 |
| **任务启动** | 开销大 | 持续运行的进程 |
| **灵活性** | 只有 Map+Reduce | 多种算子 |

### 4.2 Spark 核心概念

**Spark** 是新一代数据流引擎，通过 DAG 和内存计算解决了 MapReduce 的问题。

```mermaid
graph TD
    subgraph Spark架构["Spark 核心概念"]
        RDD[弹性分布式数据集 RDD]
        DAG[有向无环图 DAG]
        任务[任务 Task]
        Stage[阶段 Stage]

        RDD -->|转换| RDD2
        RDD2 -->|行动| 结果

        DAG --> Stage1
        DAG --> Stage2
    end

    subgraph 核心特性["Spark 特性"]
        C1[内存计算]
        C2[DAG 调度]
        C3[容错恢复]
        C4[数据分区]
    end
```

### 4.3 RDD 与 DataFrame

```mermaid
graph TD
    subgraph RDD["RDD (Resilient Distributed Dataset)"]
        R1[低层 API]
        R2[类型安全]
        R3[函数式风格]
        R4[灵活但冗长]
    end

    subgraph DataFrame["DataFrame"]
        D1[高层 API]
        D2[类似 SQL]
        D3[优化执行]
        D4[简洁易用]
    end

    subgraph Dataset["Dataset"]
        S1[类型安全]
        S2[编译时检查]
        S3[JVM 语言友好]
    end
```

### 4.4 Spark 执行流程

```mermaid
sequenceDiagram
    participant U as 用户程序
    participant D as Driver
    participant C as ClusterManager
    participant W as Worker
    participant E as Executor

    U->>D: 提交 Spark 应用
    D->>D: 构建 DAG
    D->>C: 请求资源

    C->>W: 启动 Executor
    W->>E: 启动 Executor 进程

    D->>E: 发送任务
    E->>E: 执行 Task

    E-->>D: 返回结果
    D-->>U: 输出结果
```

### 4.5 Spark 宽依赖与窄依赖

```mermaid
graph TD
    subgraph 窄依赖["窄依赖 Narrow Dependency"]
        N1[Parent RDD]
        N2[Child RDD 1]
        N3[Child RDD 2]

        N1 --> N2
        N1 --> N3
        N2 --> N4[Partition 1]
        N3 --> N5[Partition 2]
    end

    subgraph 宽依赖["宽依赖 Wide Dependency"]
        W1[RDD A]
        W2[RDD B]

        W1 -->|Shuffle| W2

        W2 --> P1[Partition 1]
        W2 --> P2[Partition 2]
        W2 --> P3[Partition 3]
    end
```

| 依赖类型 | 特点 | 适用操作 |
|---------|-----|---------|
| **窄依赖** | 父分区对应一个子分区 | map, filter |
| **宽依赖** | 父分区对应多个子分区 | groupBy, join |

---

## 五、批处理优化技术

### 5.1 数据分区与并行度

```mermaid
graph TD
    subgraph 分区策略["分区策略"]
        P1[哈希分区]
        P2[范围分区]
        P3[自定义分区]
    end

    subgraph 并行度["并行度设置"]
        D1[输入数据量]
        D2[集群资源]
        D3[任务特性]

        D1 --> P["设置分区数"]
        D2 --> P
        D3 --> P
    end
```

### 5.2 排序优化

```mermaid
graph TD
    subgraph 排序策略["排序优化"]
        S1[全局排序]
        S2[二次排序]
        S3[采样排序]

        S1 -->|"大数据量"| S11["使用分桶排序"]
        S2 -->|"按值排序"| S21["组合键排序"]
        S3 -->|"抽样确定边界"| S31["近似排序"]
    end
```

### 5.3 连接策略

```mermaid
graph TD
    subgraph 连接类型["连接类型"]
        J1[Shuffle Join]
        J2[Broadcast Join]
        J3[Sort Merge Join]
        J4[Hash Join]
    end

    J1 -->|"大表连接大表"| A1["默认策略"]
    J2 -->|"大表连接小表"| A2["广播小表"]
    J3 -->|"有序数据"| A3["排序后合并"]
    J4 -->|"内存充足"| A4["内存哈希"]
```

**连接策略选择：**

```mermaid
flowchart TD
    A[选择连接策略] --> B{数据规模}

    B -->|小表 < 10MB| C[Broadcast Join]
    B -->|大表连接大表| D{数据分布}

    D -->|已排序| E[Sort Merge Join]
    D -->|未排序| F[Shuffle Join]

    C --> G["小表广播到所有节点"]
    E --> H["排序后合并"]
    F --> I["Shuffle 后 Hash Join"]
```

### 5.4 缓存与持久化

```mermaid
graph TD
    subgraph 缓存策略["缓存策略"]
        C1[缓存热点数据]
        C2[重复使用的中间结果]
        C3[迭代计算的数据]

        C1 --> R1["加速读取"]
        C2 --> R2["避免重复计算"]
        C3 --> R3["支持迭代"]
    end

    subgraph 存储级别["存储级别"]
        S1[内存]
        S2[内存+磁盘]
        S3[仅磁盘]
    end
```

---

## 六、批处理的应用场景

### 6.1 ETL 数据处理

```mermaid
graph TD
    subgraph ETL流程["ETL 流程"]
        E[Extract 抽取] --> T[Transform 转换]
        T --> L[Load 加载]

        E --> E1["从源系统读取"]
        T --> T1["清洗、转换、聚合"]
        L --> L1["写入目标系统"]
    end

    subgraph 批处理ETL["批处理 ETL 特点"]
        P1[定时执行]
        P2[处理历史数据]
        P3[数据质量检查]
        P4[增量与全量]
    end
```

### 6.2 数据仓库

```mermaid
graph TD
    subgraph 数据仓库["数据仓库架构"]
        ODS[操作数据存储]
        DW[数据仓库]
        DM[数据集市]

        ODS -->|"ETL"| DW
        DW -->|"切片"| DM

        subgraph 分层["分层架构"]
            L1[STG 临时层]
            L2[DWD 明细层]
            L3[DWS 汇总层]
            L4[ADS 应用层]
        end
    end
```

### 6.3 机器学习训练

```mermaid
graph TD
    subgraph ML训练["批处理在 ML 中的应用"]
        D[数据准备] --> F[特征工程]
        F --> T[模型训练]
        T --> E[模型评估]

        D --> D1["数据清洗"]
        D --> D2[特征提取]
        T --> T1["批量梯度下降"]
        T --> T2[分布式训练]
    end

    subgraph 分布式训练["分布式训练策略"]
        S1[数据并行]
        S2[模型并行]
        S3[流水线并行]
    end
```

---

## 七、面试题整理

### 7.1 概念理解类 🌱

**Q1：MapReduce 的执行流程是什么？**

**答案：**

**MapReduce 执行流程：**

```mermaid
graph TD
    A[输入分片] --> B[Map 阶段]
    B --> C[Shuffle 阶段]
    C --> D[Reduce 阶段]
    D --> E[输出]

    A --> A1["InputFormat 决定分片"]
    B --> B1["每个分片一个 Map 任务"]
    C --> C1["分区、排序、合并"]
    D --> D1["相同 key 的数据聚合"]
```

**详细步骤：**

1. **输入分片**：将大文件分成固定大小的块
2. **Map 阶段**：每个 Map 任务处理一个分片，输出键值对
3. **Shuffle 阶段**：按 key 分区、排序，将数据发送到 Reduce
4. **Reduce 阶段**：对相同 key 的数据进行聚合
5. **输出**：写入 HDFS

**Q2：Spark 和 MapReduce 有什么区别？**

**答案：**

| 特性 | MapReduce | Spark |
|-----|-----------|-------|
| **编程模型** | Map + Reduce | RDD + 多种转换 |
| **中间结果** | 写 HDFS | 内存缓存 |
| **迭代计算** | 每次重跑 | 缓存 RDD |
| **容错** | 任务重试 | Lineage 重建 |
| **资源利用** | 任务级隔离 | Executor 进程 |
| **API 丰富度** | 有限 | 丰富 |

---

### 7.2 原理分析类 🌿

**Q3：Spark 的宽依赖和窄依赖有什么区别？有什么影响？**

**答案：**

**窄依赖：**
- 一个父分区只对应一个子分区
- 可以在同一个节点流水线执行
- 失败恢复只需要重新计算受影响的分区

**宽依赖：**
- 一个父分区对应多个子分区
- 需要 Shuffle，数据跨节点传输
- 失败恢复需要重新计算所有上游分区

**对 Stage 划分的影响：**

```mermaid
graph TD
    subgraph Stage划分["Stage 划分规则"]
        R1["窄依赖 → 同一个 Stage"]
        R2["宽依赖 → 新 Stage 边界"]
        R3["Shuffle → Stage 分隔符"]
    end

    D1["map → filter → map → filter"]
    D2["map → groupBy → reduce"]
    D3["↑ 这里有 Shuffle，划分 Stage"]
```

**Q4：Spark 如何实现容错？RDD 的 Lineage 是什么？**

**答案：**

**Spark 容错机制：**

```mermaid
graph TD
    subgraph 容错实现["Spark 容错机制"]
        L[Lineage 血缘]
        C[Checkpoint 检查点]
        R[任务重试]

        L -->|"依赖关系"| R1["丢失分区从上游重建"]
        C -->|"快照"| R2["避免长 lineage"]
        R -->|"失败重试"| R3["重新执行任务"]
    end
```

**RDD Lineage（血缘）：**
- 每个 RDD 记录了创建它的转换操作
- 形成一个依赖图（Lineage Graph）
- 丢失数据时，可以根据 Lineage 重新计算

```scala
// Lineage 示例
val lines = sc.textFile("hdfs://...")           // HadoopRDD
val words = lines.flatMap(_.split(" "))          // FlatMappedRDD
val pairs = words.map((_, 1))                    // MapPartitionsRDD
val counts = pairs.reduceByKey(_ + _)             // ShuffledRDD

// Lineage: HadoopRDD → FlatMappedRDD → MapPartitionsRDD → ShuffledRDD
```

---

### 7.3 对比选型类 🔧

**Q5：什么场景下应该使用批处理，什么场景下应该使用流处理？**

**答案：**

```mermaid
flowchart TD
    A[选择处理模式] --> B{数据特点}

    B -->|历史数据| C[批处理]
    B -->|实时数据| D[流处理]
    B -->|两者都要| E[Lambda 架构]

    C --> C1["日终报表"]
    C --> C2["模型训练"]
    C --> C3["全量索引"]

    D --> D1["实时监控"]
    D --> D2["告警"]
    D --> D3["在线推荐"]

    E --> E1["实时+历史结合"]
    E --> E2["批流一体"]
```

**选择建议：**

| 场景 | 推荐 | 原因 |
|-----|-----|-----|
| 日报/周报 | 批处理 | 数据完整、处理时间充裕 |
| 实时监控 | 流处理 | 延迟要求高 |
| 机器学习 | 批处理 | 需要全量数据 |
| 异常检测 | 流处理 + 批处理 | 实时 + 历史对比 |

**Q6：如何优化 Spark 作业的性能？**

**答案：**

**性能优化方向：**

```mermaid
graph TD
    subgraph 优化维度["Spark 优化维度"]
        O1[数据序列化]
        O2[内存管理]
        O3[并行度设置]
        O4[数据倾斜处理]
        O5[Shuffle 优化]
    end

    O1 --> S1["使用 Kryo 序列化"]
    O2 --> S2["调整堆内存/Off-heap"]
    O3 --> S3["合理设置分区数"]
    O4 --> S4["倾斜 key 单独处理"]
    O5 --> S5["减少 Shuffle 数据量"]
```

**具体优化措施：**

| 优化项 | 具体措施 | 效果 |
|-------|---------|-----|
| **序列化** | 使用 Kryo 替代 Java 序列化 | 减少 10x 体积 |
| **缓存** | 缓存常用 RDD | 避免重复计算 |
| **并行度** | 设置合适的分区数 | 充分利用资源 |
| **Shuffle** | 减少 Shuffle 文件 | 提升性能 |
| **广播** | 广播小表 | 避免 Shuffle |
| **数据倾斜** | 加盐、打散、聚合 | 避免热点 |

---

### 7.4 实战应用类 🔧

**Q7：设计一个批处理系统来处理日增 1TB 的日志数据**

**答案：**

**系统设计：**

```mermaid
graph TD
    subgraph 整体架构["批处理系统架构"]
        I[数据采集] --> Q[消息队列]
        Q --> H[HDFS 存储]
        H --> S[Spark 处理]
        S --> D[数据仓库]
        D --> R[报表/应用]
    end

    subgraph 技术选型["技术选型"]
        T1[采集: Flume/Filebeat]
        T2[存储: HDFS/Parquet]
        T3[处理: Spark]
        T4[调度: Airflow/Oozie]
    end
```

**关键设计点：**

| 设计点 | 方案 | 考虑因素 |
|-------|-----|---------|
| **数据分区** | 按日期/小时分区 | 查询效率 |
| **文件格式** | Parquet + Snappy | 压缩率 + 性能 |
| **调度策略** | 每日凌晨执行 | 资源空闲期 |
| **容错机制** | 检查点 + 重试 | SLA 保证 |
| **监控告警** | 任务执行监控 | 问题及时发现 |

---

## 八、实践要点

### 8.1 批处理作业设计原则

```mermaid
graph TD
    A[批处理设计原则] --> B[输入优化]
    A --> C[计算优化]
    A --> D[输出优化]
    A --> E[资源优化]

    B --> B1["合理分区"]
    B --> B2["使用列式存储"]
    C --> C1["减少 Shuffle"]
    C --> C2["缓存热点数据"]
    D --> D1["批量写入"]
    D --> D2["适当压缩"]
    E --> E1["动态资源分配]
    E --> E2["预估资源需求"]
```

### 8.2 常见问题与解决方案

| 问题 | 原因 | 解决方案 |
|-----|-----|---------|
| **数据倾斜** | Key 分布不均 | 加盐、打散 |
| **内存溢出** | 分区过大 | 增加分区数 |
| **任务卡住** | 数据倾斜/资源不足 | 监控定位 |
| **执行慢** | 并行度不足 | 调整分区数 |
| **Shuffle 量大** | 连接/聚合不当 | 使用广播 |

### 8.3 批处理与流处理的关系

```mermaid
graph TD
    subgraph 两种处理["批处理与流处理"]
        B[批处理: 有界数据] --> L[Lambda 架构]
        S[流处理: 无界数据] --> L

        L -->|"批处理层"| H[HDFS]
        L -->|"速度层"| R[实时计算]
        L -->|"服务层"| V[结果合并]
    end

    subgraph Kappa架构["Kappa 架构（批流一体）"]
        K[统一流处理]
        K --> B["批量处理 = 流的切片"]
        K --> S["实时处理 = 流的部分"]
    end
```

---

## 九、扩展阅读

### 9.1 必读论文

| 论文 | 作者 | 年份 | 贡献 |
|-----|-----|-----|-----|
| MapReduce | Dean, Ghemawat | 2004 | MapReduce 模型 |
| Spark | Zaharia et al. | 2010 | Spark 内存计算 |
| Dataflow | Google | 2015 | Dataflow 模型 |

### 9.2 推荐资源

- Spark 官方文档：spark.apache.org
- 《Spark: The Definitive Guide》
- Hadoop 官方文档

### 9.3 实践项目

- 实现 WordCount（MapReduce + Spark）
- 设计 ETL 管道
- 搭建 Spark 集群进行大数据分析

---

## 十、本章小结

### 核心收获

1. **批处理的本质**
   - 处理有限数据集
   - 高吞吐、高容错
   - 适用于离线分析

2. **MapReduce 模型**
   - Map + Reduce 简单模型
   - Shuffle 实现数据分发
   - 容错通过任务重试

3. **Spark 的改进**
   - 内存计算
   - DAG 执行计划
   - 丰富的 API

4. **批处理优化**
   - 合理设置并行度
   - 减少 Shuffle
   - 处理数据倾斜

### 概念地图

```mermaid
mindmap
  root((批处理))
    计算模型
      MapReduce
      Spark RDD
      Dataflow
    核心概念
      分区
      Shuffle
      容错
    优化技术
      缓存
      广播
      序列化
```

### 下一章预告

第 11 章将探讨**流处理**，了解如何处理实时数据流：
- 事件时间处理
- 窗口计算
- 流处理框架
- 批流一体

---

## 附录 A：MapReduce vs Spark 对比

| 特性 | MapReduce | Spark |
|-----|-----------|-------|
| 编程模型 | Map + Reduce | 转换 + 行动 |
| 中间结果 | HDFS | 内存 |
| 容错 | 任务重试 | Lineage |
| 迭代计算 | 低效 | 高效 |
| 资源管理 | YARN | YARN/K8s |
| 语言支持 | Java | Scala/Java/Python/R |

## 附录 B：Spark RDD 转换操作

| 操作类型 | 转换 | 说明 |
|---------|-----|-----|
| **转换** | map | 元素级转换 |
| **转换** | flatMap | 展平转换 |
| **转换** | filter | 过滤 |
| **转换** | groupByKey | 按 key 分组 |
| **转换** | reduceByKey | 按 key 聚合 |
| **行动** | collect | 收集到 driver |
| **行动** | count | 计数 |
| **行动** | saveAsTextFile | 保存 |

---

*文档生成时间：2024-01-08*
*基于《Designing Data-Intensive Applications》第10章*
