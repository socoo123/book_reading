# 第三章：创造大局观

> **原书标题**：Creating the Big Picture
> **所属**：Part I - The Big Picture（大局观）
> **核心问题**：当组织缺少技术方向时，你如何创建技术愿景或技术战略？从构思到落地的完整方法论是什么？

---

## 本章结构

```mermaid
flowchart TB
    A["引子：SockMatcher的困境<br/>── 三个人对同一系统有三种计划"] --> B["愿景 vs 策略<br/>── 两种不同的文档"]
    B --> C["阶段一：方法论（Approach）<br/>── 拥抱无聊、找赞助人、建核心组、设范围"]
    C --> D["阶段二：写作（Writing）<br/>── 迭代循环：写→访谈→思考→决策→对齐"]
    D --> E["阶段三：发布（Launch）<br/>── 让文档变成'组织的'而非'你的'"]
    E --> F["案例研究：SockMatcher 全流程"]

    style A fill:#e8f0fe,stroke:#1a73e8
    style B fill:#fef7e0,stroke:#f9ab00
    style C fill:#e6f4ea,stroke:#34a853
    style D fill:#f3e8fd,stroke:#9334e6
    style E fill:#fce8e6,stroke:#ea4335
    style F fill:#fff3e0,stroke:#e65100
```

---

## 引子：为什么需要"大局观"？

作者讲了一个故事：在一次全员大会上，有人提问了一个关于频繁宕机的关键系统（SystemX）的问题。她准备回答时，同时收到了**三条完全矛盾的私信**：

```mermaid
graph TB
    Q["关于 SystemX 的问题"]

    Q --> DM1["私信1：'请大家放心<br/>我们正在扩充团队<br/>增加副本来帮助它扩展<br/>不会再有更多宕机了'"]

    Q --> DM2["私信2：'太好了有人问！<br/>SystemX 已经被弃用了<br/>请大家准备迁移走'"]

    Q --> DM3["私信3：'嗨，我成立了一个<br/>工作组来探索如何发展 SystemX<br/>下个季度会公布计划'"]

    BOTTOM["三个完全不同的方向<br/>——都很合理——但组织没有共识<br/>这就是缺少大局观的典型症状"]

    DM1 & DM2 & DM3 --> BOTTOM

    style BOTTOM fill:#fce8e6,stroke:#ea4335
```

> **当缺乏明确的技术方向时，每个团队各自为政，做出局部合理但全局矛盾的决策。** 这就是你需要创造"大局观"的信号。

---

## 为什么去中心化决策会出问题？

```mermaid
graph TD
    PROBLEM["每个团队自己做决策"]

    PROBLEM --> P1["公地悲剧<br/>──────<br/>每个团队追求自身最优<br/>不协调 → 结果对所有人都差<br/>又是那个 local maximum"]

    PROBLEM --> P2["共享问题无人认领<br/>──────<br/>没有哪个团队有权限<br/>或动机去独自修复<br/>共享基础设施的问题"]

    PROBLEM --> P3["上下文缺失<br/>──────<br/>做决策的人和承受后果的人<br/>可能是不同的人<br/>或者决策者缺少足够信息"]

    RESULT["结果：大决策被不断推迟<br/>团队根据传言或偏好做局部选择<br/>并将这些选择嵌入代码中<br/>拖得越久越难修正"]

    P1 & P2 & P3 --> RESULT

    style RESULT fill:#fce8e6,stroke:#ea4335
```

---

## 愿景 vs 策略：两种不同的文档

```mermaid
graph LR
    subgraph VISION["技术愿景 (Technical Vision)"]
        direction TB
        V1["描述未来状态<br/>──────<br/>'当所有大问题都解决后<br/>世界会是什么样子？'"]
        V2["像北极星<br/>──────<br/>不做所有决策<br/>但消除冲突和模糊<br/>让每个人能自信地<br/>选择自己的路径"]
        V3["形式多样<br/>──────<br/>• 高层价值观和目标<br/>• 决策原则<br/>• 已做决策汇总<br/>• 架构图<br/>• 一句话愿景宣言<br/>• 20页详细文章"]
    end

    subgraph STRATEGY["技术策略 (Technical Strategy)"]
        direction TB
        S1["描述行动计划<br/>──────<br/>'我们如何达到目标<br/>克服路上的障碍？'"]
        S2["Rumelt 策略核心<br/>──────<br/>1. 诊断 (Diagnosis)<br/>2. 指导方针 (Guiding Policy)<br/>3. 协调行动 (Coherent Actions)"]
        S3["形式多样<br/>──────<br/>• 优先级列表<br/>• 权衡决策及理由<br/>• 具体项目和成功标准<br/>• 可以1-2页或60页"]
    end

    VISION <-->|"互相配合<br/>愿景说去哪<br/>策略说怎么去"| STRATEGY

    style VISION fill:#e8f0fe,stroke:#1a73e8
    style STRATEGY fill:#fef7e0,stroke:#f9ab00
```

### 文档层级关系

```mermaid
graph TB
    BIZ["业务/产品愿景和策略"] --> EV["工程愿景"]
    VAL["价值观/原则"] --> EV
    BIZ --> ES["工程策略"]
    EV --> OV["组织愿景"] --> TV["团队愿景"]
    ES --> OS["组织策略"] --> TS["团队策略"]
    ES --> ER["工程路线图"] --> OR["组织路线图"] --> TR["团队路线图"]
    OR --> PROJ["各种规模的项目"]
    PROJ --> CODE["代码"]
    PROJ --> CULTURE["文化"]
    PROJ --> PROCESS["流程"]
    PROJ --> RFC["RFC / 设计文档"]

    NOTE["你可以从任何一层开始<br/>不需要时不要创建<br/>──────<br/>⚠️ 如果每个人已经对齐<br/>不需要文档就能达成共识<br/>那就不需要写这个文档"]

    style NOTE fill:#fef7e0,stroke:#f9ab00
    style BIZ fill:#e8f0fe,stroke:#1a73e8
```

### Rumelt 策略核心（Strategy Kernel）详解

来自 Richard Rumelt 的经典著作 *Good Strategy/Bad Strategy*：

```mermaid
graph TB
    subgraph KERNEL["策略核心 (Strategy Kernel)"]
        direction TB
        DIAG["1. 诊断 (Diagnosis)<br/>──────<br/>'到底发生了什么？'<br/>从混乱现实中提取模式<br/>用隐喻或心智模型<br/>让问题变得可理解<br/>⚠️ 要比现实更简单<br/>这很难但很必要"]

        GUIDE["2. 指导方针 (Guiding Policy)<br/>──────<br/>'我们选择什么方向？'<br/>简短明确<br/>Rumelt: '一个路标<br/>标记前进方向'<br/>让后续决策变得更容易"]

        ACTION["3. 协调行动 (Coherent Actions)<br/>──────<br/>'我们具体做什么？'<br/>不只是技术行动<br/>可能包括组织变化、新流程<br/>要具体说明做什么和不做什么<br/>利用你的优势创造正反馈循环"]
    end

    DIAG --> GUIDE --> ACTION

    style KERNEL fill:#f8f9fa,stroke:#333
    style DIAG fill:#e8f0fe,stroke:#1a73e8
    style GUIDE fill:#fef7e0,stroke:#f9ab00
    style ACTION fill:#e6f4ea,stroke:#34a853
```

> **关键原则：策略必须现实。** 一个无法在你的组织中获得资源和人员支持的策略，就是浪费你的时间。策略应该利用你的优势——而不是描述"完美世界中别人会做什么"。

---

## 你真的需要写愿景/策略文档吗？

```mermaid
graph TB
    NEED["你真的需要吗？"]

    NEED -->|"不需要"| NO1["如果大家已经对齐<br/>不写文档也没问题<br/>──────<br/>也许一个设计文档的目标部分<br/>或一个 PR 描述就够了"]

    NEED -->|"可能不需要完整文档"| NO2["如果只是卡在一个<br/>具体的架构决策上<br/>──────<br/>直接做那个决策就行<br/>不需要花时间在哲学上"]

    NEED -->|"需要"| YES["如果缺乏方向导致<br/>团队减速、方向矛盾<br/>决策被反复推迟<br/>──────<br/>那就需要<br/>但选择适合组织的形式<br/>不要做比需要更多的东西"]

    style YES fill:#34a853,color:#fff
```

---

## 阶段一：方法论（The Approach）

创造愿景/策略是一个大项目。需要大量准备、迭代和对齐。**获得大家的认同不是真正工作之外的苦差事——认同本身就是工作。**

### 第一步：拥抱"无聊"的想法

```mermaid
graph TB
    BORING["拥抱无聊的想法"]

    BORING --> B1["现实不是星际迷航<br/>──────<br/>初入行时你以为资深工程师<br/>都是在关键时刻喊出<br/>'用反相位脉冲！'<br/>然后一键拯救世界"]

    BORING --> B2["现实是：好的想法已经存在<br/>──────<br/>如果周围有资深人<br/>好点子早就满天飞了<br/>缺的不是洞察力<br/>而是把大家对齐的能力"]

    BORING --> B3["好策略本质上是无聊的<br/>──────<br/>Camille Fournier (The Manager's Path):<br/>'好策略相当无聊<br/>如果你写出的东西很有趣<br/>那很可能是错的<br/>策略的核心是综合'"]

    BORING --> B4["你的价值在于：<br/>权衡所有方案<br/>决定做什么和不做什么<br/>对齐所有人<br/>勇敢地做出（可能错误的）决策"]

    style BORING fill:#fef7e0,stroke:#f9ab00
```

> Will Larson 的建议："如果你忍不住要在过程中加入你最精彩的点子，可以把它们放在预备工作中。把你所有最佳想法写进一个大文档里，删掉它，再也不提。现在你的脑子清净了，可以干正事了。"

### 第二步：如果别人已经在做，加入他们

```mermaid
graph TB
    JOIN["如果有人已经在做类似的事"]

    JOIN --> J1["共同领导 (Share the Lead)<br/>──────<br/>正式分工<br/>一个人做整体<br/>另一个做内部倡议<br/>轮流当主要作者"]

    JOIN --> J2["做最佳配角 (Follow their Lead)<br/>──────<br/>放下自尊<br/>用你的深度技术知识<br/>填补他们的短板<br/>在他们进不去的房间<br/>为计划代言"]

    JOIN --> J3["离开 (Step Away)<br/>──────<br/>如果项目不需要你<br/>开心地支持他们<br/>然后去找需要你的地方"]

    WARNING["⚠️ 不要玩'猿猴游戏'<br/>──────<br/>晋升体系让工程师觉得<br/>必须'赢得'技术方向<br/>把别人的想法视为威胁<br/>→ 这对合作和组织有毒<br/><br/>Robert Konigsberg (Google):<br/>'不要因为想法来自你的脑子<br/>就认为它是最好的'"]

    JOIN --> WARNING

    style WARNING fill:#fce8e6,stroke:#ea4335
```

### 第三步：获取赞助人（Sponsor）

```mermaid
graph TB
    SPONSOR["为什么需要赞助人？"]

    SPONSOR --> S1["任何大努力都需要<br/>高层支持才能成功<br/>──────<br/>没有赞助人的愿景<br/>可以开始但很难落地"]

    SPONSOR --> S2["赞助人能做什么<br/>──────<br/>• 证明这是组织的工作<br/>  不只是你个人的<br/>• 在委员会僵持时做裁判<br/>• 指定DRI（直接责任人）<br/>• 设定成功标准<br/>• 为工作争取资源和人员"]

    SPONSOR --> S3["如何获取赞助人<br/>──────<br/>找到这个问题和他们目标的交集<br/>他们也有'永远为真的目标'：<br/>让团队更高效、更开心<br/><br/>练习你的'电梯演讲'<br/>如果50个字说不清<br/>可能完全说不清"]

    SPONSOR --> S4["维护赞助关系<br/>──────<br/>Sean Rees (Reddit Principal):<br/>'最大的错误之一是<br/>不维护你的高管赞助关系<br/>赞助可以开始然后逐渐消退…'<br/>定期更新、确认仍在同一页上"]

    style SPONSOR fill:#1a73e8,color:#fff
```

> **Staff+ 工程师能当赞助人吗？** 通常不行。赞助人需要有权决定组织投入时间和人员在什么上面——这通常是 Director 或 VP 的权力。

### 第四步：组建核心组

```mermaid
graph TB
    CORE["核心组的组建"]

    CORE --> C1["规模：2-4人<br/>──────<br/>包括你在内<br/>设定时间承诺<br/>（如每周8-12小时）<br/>来排除'游客'"]

    CORE --> C2["谁应该在组里？<br/>──────<br/>拉出你的地形图<br/>谁在你这边？<br/>谁会反对？<br/>──────<br/>反对者：要么确保<br/>你有足够支持压过他们<br/>要么从一开始就拉进来<br/>理解他们反对的原因"]

    CORE --> C3["让他们真正参与<br/>──────<br/>让核心组有想法、有驱动力<br/>如果你以后要做裁判<br/>从一开始就明确你的Lead角色<br/>不要事后突然拉级别"]

    CORE --> C4["更广泛的盟友<br/>──────<br/>核心组之外<br/>还要采访并更新<br/>更广泛的有影响力的人<br/>邀请他们评论早期草稿"]

    style CORE fill:#e6f4ea,stroke:#34a853
```

### 第五步：设定范围

```mermaid
graph TB
    SCOPE["设定范围需要考虑的问题"]

    SCOPE --> SC1["范围多大？<br/>──────<br/>影响全部工程？<br/>一个团队？一套系统？<br/>要覆盖到真正解决问题<br/>但不超过你的影响力范围"]

    SCOPE --> SC2["务实<br/>──────<br/>如果你的愿景依赖于<br/>你完全控制不了的事<br/>（如换CEO）<br/>那就是 magical thinking<br/>围绕固定约束做计划"]

    SCOPE --> SC3["可能被推翻<br/>──────<br/>更高层的计划<br/>可能随时打乱你的<br/>做好重新审视的准备"]

    SCOPE --> SC4["确保可实现<br/>──────<br/>前方有多少未解问题？<br/>问做过类似事情的人<br/>'你觉得这行得通吗？'<br/>如果不行你有五个选项"]

    style SCOPE fill:#f3e8fd,stroke:#9334e6
```

**如果评估后发现问题不可解或不该由你解，你有五个选择：**

| 选项 | 说明 |
|---|---|
| 自欺欺人硬上 | 可以但不推荐 |
| 找到有你缺失技能的人合作 | 让他们领导或给你一部分来磨练 |
| 缩小范围重新界定问题 | 添加固定约束，换一个能解的问题 |
| 接受没人会写这个，公司可能也没事 | 去做别的 |
| 接受没人会写这个，公司不会没事 | 更新你的简历 |

### 第六步：正式立项

在开始之前的最终检查清单：

```mermaid
graph TB
    CHECK["开始前的检查清单"]

    CHECK --> CK1["✅ 我们确实需要这个"]
    CHECK --> CK2["✅ 我知道方案会是<br/>无聊且显而易见的"]
    CHECK --> CK3["✅ 没有已存在的努力<br/>（或我已加入了）"]
    CHECK --> CK4["✅ 有组织支持"]
    CHECK --> CK5["✅ 我们就创建什么<br/>类型的文档达成了一致"]
    CHECK --> CK6["✅ 问题是可解的（由我）"]
    CHECK --> CK7["✅ 以上我都没有自欺欺人"]

    style CHECK fill:#e6f4ea,stroke:#34a853
```

> 如果你有拖延或分心的倾向，正式立项——设置启动文档、里程碑、时间线和进展汇报机制——将帮你保持在正轨上。

---

## 阶段二：写作（The Writing）

### 写作循环

这不是线性过程，而是一个反复迭代的循环：

```mermaid
graph TB
    IDEAS["你的初始想法"] --> WRITE["写作"]
    WRITE --> THINK["思考、处理、<br/>综合、决策"]
    THINK --> TALK["和人交流：<br/>采访、对齐"]
    TALK --> THINK
    THINK --> WRITE
    WRITE --> READY["准备好广泛分享"]

    style IDEAS fill:#ff9800,color:#fff
    style READY fill:#ff9800,color:#fff
    style WRITE fill:#e8f0fe,stroke:#1a73e8
    style THINK fill:#fef7e0,stroke:#f9ab00
    style TALK fill:#e6f4ea,stroke:#34a853
```

> **设置时间框！** 信息永远不会完美，但会出现收益递减。当迭代开始没有新发现时，停下来收尾。不怕不完美——你可以也应该定期重新审视文档。

### 初始想法：你需要问自己的问题

```mermaid
graph TB
    QUESTIONS["写之前问自己的问题"]

    QUESTIONS --> Q1["已经存在什么文档？<br/>──────<br/>公司愿景、产品方向？<br/>比你范围更小的团队文档？<br/>继承更大范围的约束<br/>理解你的变更可能<br/>打乱更小范围的文档"]

    QUESTIONS --> Q2["什么需要改变？<br/>──────<br/>团队被依赖阻塞？→ 强调自治<br/>新功能发布慢？→ 迭代速度<br/>产品老是挂？→ 可靠性<br/>想大：部署要一天？<br/>不要说'减半'<br/>要说'20分钟'<br/>→ 激发更勇敢的想法"]

    QUESTIONS --> Q3["什么已经很棒了？<br/>──────<br/>确保未来状态<br/>包含保留当前的优势<br/>不要无意中牺牲掉它们"]

    QUESTIONS --> Q4["什么真正重要？<br/>──────<br/>你的愿景/策略会影响很多人<br/>不要浪费他们的时间<br/>在不重要的事上<br/>如果要大家做昂贵迁移<br/>终点要有宝藏"]

    QUESTIONS --> Q5["未来的你会后悔什么？<br/>──────<br/>想象2-3年后<br/>更老更聪明的你<br/>哪些问题每个季度<br/>都在恶化？<br/>'给未来发一块饼干'"]

    style QUESTIONS fill:#1a73e8,color:#fff
```

### 写作方式：两种起草策略

```mermaid
graph LR
    subgraph LEADER["领导者起草"]
        direction TB
        L1["你写第一稿供讨论"]
        L2["✅ 给文档统一的声音和关注"]
        L3["⚠️ 初稿会受你的偏见影响"]
        L4["⚠️ 审阅者倾向于顺从<br/>资深人写的内容"]
        L5["缓解：事先大量讨论<br/>标记随意做的决策为可讨论<br/>'Strong opinions weakly held'<br/>只有确保安全时才生效"]
    end

    subgraph MULTI["多人起草汇总"]
        direction TB
        M1["每人写独立草稿<br/>然后汇总"]
        M2["✅ 获取无偏见的多元意见"]
        M3["⚠️ 作者可能情感投入<br/>变成互相批评而非合作"]
        M4["缓解：事先明确<br/>所有草稿只是输入<br/>最终只有一份文档<br/>没有'赢家'"]
    end

    style LEADER fill:#e8f0fe,stroke:#1a73e8
    style MULTI fill:#fef7e0,stroke:#f9ab00
```

### 采访：不要只和你认识的人聊

```mermaid
graph TB
    INTERVIEW["采访技巧"]

    INTERVIEW --> I1["广泛采访<br/>──────<br/>不只是朋友和近同事<br/>找领导、影响者、<br/>一线工程师、其他领域的人"]

    INTERVIEW --> I2["好的开放问题<br/>──────<br/>• '我们在做X的计划，<br/>  什么最重要？'<br/>• '和Y团队合作感觉怎样？'<br/>• '如果有魔法棒，<br/>  你会改变什么？'"]

    INTERVIEW --> I3["后期更精准<br/>──────<br/>• 分享半成品让他们反应<br/>• 描述你的思考求反馈<br/>• 展示稻草人方案<br/>  '这靠谱吗？'"]

    INTERVIEW --> I4["永远以这句话结束：<br/>──────<br/>'还有什么我应该问的？<br/>有什么重要的我漏掉了吗？'"]

    style INTERVIEW fill:#e6f4ea,stroke:#34a853
```

### 思考时间：警惕"解决方案思维"

> 工程师有一个常见的心智陷阱：从比较**问题**开始，不知不觉变成讨论**应该用什么技术**。Cindy Sridharan 警告说："一个有魅力的工程师可以成功地把自己的宠物项目卖成组织级倡议。" 当你审视要做的工作时，反复问"为什么？"直到你确信解释路径能映射到你要达成的目标。

### 做决策

```mermaid
graph TB
    DECIDE["做决策的关键原则"]

    DECIDE --> D1["权衡取舍<br/>──────<br/>每个方案都有优缺点<br/>提前想好你愿意接受<br/>哪些缺点<br/><br/>用 'even over' 语句：<br/>'我们优化易用性<br/>even over 性能'"]

    DECIDE --> D2["建立粗略共识<br/>──────<br/>IETF 模型：<br/>不追求完美共识<br/>追求'没有重大反对'<br/><br/>不问'大家都同意A吗？'<br/>而问'谁不能接受A？'"]

    DECIDE --> D3["不做决策本身就是决策<br/>——而且通常是最差的<br/>──────<br/>不决策 = 维持现状 + 不确定性<br/>下游决策都要为所有可能性对冲<br/><br/>你通常不需要'最佳'决策<br/>只需要'足够好'的决策<br/>如果卡住：<br/>'用两周研究存储方案<br/>然后在时间到时做选择'"]

    DECIDE --> D4["展示你的工作<br/>──────<br/>记录你考虑的权衡<br/>如何做出最终决策<br/>不回避缺点<br/>坦诚解释为什么<br/>这仍是正确的路<br/><br/>→ 减少每次有新人加入时<br/>重新审判决策的风险"]

    style DECIDE fill:#f3e8fd,stroke:#9334e6
```

### 保持对齐

```mermaid
graph TB
    ALIGN["保持对齐的策略"]

    ALIGN --> A1["定期更新赞助人<br/>──────<br/>不要发20页草稿<br/>而是带着整理好的摘要<br/>在关键检查点对齐：<br/>1. 诊断完成后<br/>2. 指导方针选定后<br/>3. 行动方案确定后"]

    ALIGN --> A2["保持合理<br/>──────<br/>Overton Window（奥弗顿窗口）：<br/>在任何时候只有一定范围的<br/>想法被认为是可接受的<br/>——不要提超出窗口的想法<br/>你会失去信誉"]

    ALIGN --> A3["根回し (Nemawashi)<br/>──────<br/>丰田生产系统原则之一：<br/>提前分享信息铺路<br/>让决策在正式做出时<br/>已经有共识<br/><br/>'不要在没有票的时候投票'"]

    ALIGN --> A4["打磨你的故事<br/>──────<br/>愿景/策略的价值<br/>取决于你讲述它的故事<br/><br/>故事要：<br/>• 易懂 (comprehensible)<br/>• 相关 (relatable)<br/>• 舒适 (comfortable)<br/><br/>一句话口号很有用：<br/>'Cloud only 2020'"]

    style ALIGN fill:#1a73e8,color:#fff
```

#### 关于讲故事的三个要求

```mermaid
graph TB
    STORY["好的故事"]

    STORY --> COMP["易懂<br/>──────<br/>短小、连贯<br/>比一堆任务清单有力<br/>人们需要能复述它<br/>——你不在时也能影响决策<br/>避免术语<br/>考虑用 persona 来描述<br/>使用前后对比"]

    STORY --> REL["相关<br/>──────<br/>宝藏对你有吸引力<br/>但对别人可能没有<br/>'我们团队会解决最烦人的问题<br/>过上快乐的生活<br/>还能吃冰淇淋'<br/>——对你的团队很诱人<br/>但如果需要别的团队帮忙<br/>要展示对他们的好处"]

    STORY --> COMF["舒适<br/>──────<br/>在Overton Window之内<br/>人们需要在A点<br/>你才能讲从A到B的故事<br/>如果他们离A很远<br/>先把他们带到A<br/>再讲下一步"]

    style STORY fill:#e6f4ea,stroke:#34a853
```

### 创建终稿

> 不是所有人都会兴奋地审阅你的文档。很多人会让它永远留在标签页里。**让文档易读**：图片、要点、大量留白。考虑用"人物角色"（persona）来描述变化对真实人的影响。你可能需要多种格式：详细文档 + 幻灯片 + 一页纸概要。

---

## 阶段三：发布（The Launch）

写完文档只是一半——你需要让它从"你的想法"变成"组织的方向"。

```mermaid
graph TB
    LAUNCH["发布三步走"]

    LAUNCH --> L1["让文档官方化<br/>──────<br/>获得最高权限者的背书<br/>VP/Director的公开支持<br/>在全员大会上引用<br/>放在内部官方网站上<br/>关闭评论、移除TODO<br/>如果能以领导名义署名<br/>比只有工程师名字更有权重"]

    LAUNCH --> L2["让工作真正发生<br/>──────<br/>文档提出的新项目需要人<br/>需要真正的预算和 headcount<br/>和赞助人讨论<br/>如何在常规规划流程中<br/>获得资源<br/><br/>你可能亲自执行<br/>也可能交接——<br/>但留在项目中<br/>保持动力和清晰度"]

    LAUNCH --> L3["保持新鲜<br/>──────<br/>发布不意味着停止思考<br/>业务方向可能变化<br/>你可能发现自己选错了<br/>准备好重新审视<br/>（一年后或更早）<br/>解释什么变了<br/>更新它、讲一个新故事"]

    style LAUNCH fill:#ff6b6b,color:#fff
```

---

## 案例研究：SockMatcher 全流程

贯穿全章的虚构案例展示了完整的愿景/策略创建流程。

### SockMatcher 是什么？

一家配对单只袜子的创业公司，使用 ML 匹配算法。后来扩展到手套、纽扣，推出外部 API (SaaaP - Sock analysis as a Platform)。架构是围绕一个单体的，所有东西共享一个数据库。现在要扩展到"食品容器+盖子"——但现有架构撑不住了。

### 案例中的关键教训

```mermaid
graph TB
    LESSONS["SockMatcher 案例的关键教训"]

    LESSONS --> LS1["了解之前为什么失败<br/>──────<br/>Pierre：闭门造车3个月<br/>做了完美技术方案<br/>但没人买账<br/><br/>Geneva：建了大联盟<br/>但没有裁判<br/>大家争论不下就散了<br/><br/>教训：你的方法必须<br/>既有技术深度<br/>又有组织可行性"]

    LESSONS --> LS2["先做诊断，再做决策<br/>──────<br/>每个人都已有解决方案<br/>'微服务！''分库分表！'<br/>但没人退一步<br/>理解真正的问题是什么<br/><br/>真正的诊断：<br/>'每次新功能都需要<br/>在共享组件中做复杂逻辑修改<br/>→ 系统需要支持更多匹配项<br/>和更多团队<br/>而不至于让开发停滞'"]

    LESSONS --> LS3["选择焦点，接受不做的事<br/>──────<br/>API版本化？→ 不做<br/>登录系统重构？→ 不做<br/>匹配算法升级？→ 不做<br/><br/>这些都是真实问题<br/>但现在不是时候<br/>写下你看到它们<br/>但选择不追——<br/>这减少'你没看到问题X'<br/>的质疑"]

    LESSONS --> LS4["指导方针要实际<br/>──────<br/>'拆成微服务'<br/>可能是理想方案<br/>但本公司没这个能力<br/>要3年还不能发新产品<br/><br/>实际指导方针：<br/>'让 billing 和 personalization<br/>变得容易且安全集成'<br/>→ 模块化、自助服务<br/>而非完全重写"]

    LESSONS --> LS5["策略不会让所有人满意<br/>但能让所有人前进<br/>──────<br/>有人觉得无聊<br/>有人觉得你选错了<br/>有人还在争论微服务<br/><br/>但：<br/>正面声音更响<br/>OKR给了官方背书<br/>盟友从一开始就参与<br/>连之前抵触的Jody团队<br/>也主动提供了帮助"]

    style LESSONS fill:#f8f9fa,stroke:#333
```

---

## 本章总结

```mermaid
graph TB
    subgraph RECAP["第三章核心要点"]
        direction TB
        R1["技术愿景描述未来状态<br/>技术策略描述行动计划"]
        R2["这类文档通常是集体努力<br/>核心组小(2-4人)<br/>但需要广泛的信息和支持"]
        R3["提前就有计划<br/>让文档变成现实<br/>通常需要高管赞助人"]
        R4["刻意选择文档类型和范围"]
        R5["写作是迭代的<br/>采访、思考、决策、对齐<br/>不断循环，需要时间"]
        R6["你的愿景/策略<br/>只有你能讲好那个故事<br/>才有价值"]
    end

    style RECAP fill:#f8f9fa,stroke:#333
```

> **一句话总结：创造大局观不是天才的灵光乍现，而是系统性的组织对齐工作——找赞助人、组核心组、做诊断、做权衡、做决策、讲故事、让文档变成组织的方向。好策略通常是无聊的，价值在于它让所有人朝同一个方向前进。**

---

**上一章**：[[ch2_三张地图]] —— 三张地图帮你理解环境、组织和方向
**下一章**：[[ch4_有限的时间]] —— Part II 开始：如何选择工作、管理时间和信誉资本
