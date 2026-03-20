# 第五章：领导大项目

> **原书标题**：Leading Big Projects
> **所属**：Part II - Execution（执行力）
> **核心问题**：作为 Staff 工程师，如何领导持续数月、跨多个团队的大型技术项目？从启动到交付的完整方法论是什么？

---

## 本章结构

```mermaid
flowchart TB
    A["什么是伟大的项目领导者？<br/>── 不是天才，是坚持、勇气和沟通"] --> B["项目的一生<br/>── 启动 → 驾驶 → 收尾"]
    B --> C["项目启动<br/>── 处理不确定感、构建上下文、<br/>搭建结构"]
    C --> D["驾驶项目<br/>── 探索、厘清、设计、编码、沟通"]
    D --> E["导航<br/>── 遇到障碍时怎么办"]

    style A fill:#fef7e0,stroke:#f9ab00
    style B fill:#e8f0fe,stroke:#1a73e8
    style C fill:#e6f4ea,stroke:#34a853
    style D fill:#f3e8fd,stroke:#9334e6
    style E fill:#fce8e6,stroke:#ea4335
```

---

## 核心前提：什么造就了伟大的项目领导者？

```mermaid
graph TB
    GREAT["伟大的项目领导者"]

    GREAT --> G1["不是天才<br/>──────<br/>也许偶尔需要灵光一闪<br/>但通常项目困难不是因为<br/>技术边界需要突破"]

    GREAT --> G2["是因为模糊<br/>──────<br/>方向不明确<br/>人际关系复杂<br/>遗留系统行为不可预测<br/>涉及大量团队"]

    GREAT --> G3["需要坚持、勇气<br/>和愿意与人交流<br/>──────<br/>这才是Staff工程师<br/>被放到这个位置的原因"]

    GREAT --> G4["你负责整个问题<br/>──────<br/>包括团队之间的缝隙<br/>和'不属于任何人'的部分<br/>项目中没人向你汇报<br/>但你要对结果负责"]

    style GREAT fill:#1a73e8,color:#fff
```

---

## 一、项目启动：面对不确定感

### 感到不知所措是正常的

```mermaid
graph TB
    OVERWHELM["感到不知所措"]

    OVERWHELM --> O1["可能的情况<br/>──────<br/>你加入一个已有历史的项目<br/>有无数决策、人际动态和文档<br/>或者项目是新的<br/>但只有一堆邮件和白板涂鸦<br/>甚至只有Director的一句话<br/>'我们要做这个'"]

    OVERWHELM --> O2["这很正常<br/>──────<br/>Polina Giralt:<br/>'那种不适感叫做学习'<br/>管理不适感本身就是一种技能"]

    OVERWHELM --> O3["冒名顶替综合症<br/>──────<br/>觉得自己不够格<br/>怕搞砸、怕让人失望<br/>George Mauer:<br/>'99%的人不比你更懂<br/>该怎么做'<br/>——所有人都在摸着走"]

    OVERWHELM --> O4["核心心态<br/>──────<br/>'困难就是重点'<br/>如果不乱不难<br/>就不需要你了<br/>你就是那个<br/>勇敢到去犯错并承认的人<br/>一个错误不会毁掉你<br/>十个也不会"]

    style OVERWHELM fill:#fef7e0,stroke:#f9ab00
```

### 五件事让项目不那么可怕

```mermaid
graph TB
    FIVE["让项目不那么可怕的五件事"]

    FIVE --> F1["给自己创建一个锚点文档<br/>──────<br/>作者的方法：<br/>创建一个私人文档<br/>充当大脑的外部存储<br/>放不确定的事、谣言、线索、<br/>待跟进的人、待办清单<br/>不确定做什么时<br/>回到这个文档找方向"]

    FIVE --> F2["和项目赞助人谈话<br/>──────<br/>了解他们想从项目中得到什么<br/>带着你写的目标和成功标准去<br/>问他们是否同意<br/>厘清你的角色<br/>以及向谁汇报更新"]

    FIVE --> F3["决定谁来承接你的不确定感<br/>──────<br/>初级工程师不是对象！<br/>他们需要你提供安全和稳定<br/>找你的管理者、导师<br/>或Staff同侪组倾诉<br/>选择会倾听和验证的人<br/>而非只想替你解决问题的人"]

    FIVE --> F4["给自己一个小胜利<br/>──────<br/>跟某人聊聊<br/>画一张图<br/>写一份文档<br/>描述问题给别人听<br/>利用'刚来不懂'的窗口期<br/>大量提问——<br/>以后会更难开口"]

    FIVE --> F5["用你的优势<br/>──────<br/>擅长代码？先读代码<br/>擅长交流？先约人聊<br/>擅长阅读？先看文档<br/>你最舒服的入口<br/>不会给你全部答案<br/>但能让大脑相信<br/>'这只是又一个项目'"]

    style FIVE fill:#e6f4ea,stroke:#34a853
```

---

### 构建上下文：用三张地图

回到第二章的框架，项目开始时你需要填充三张地图：

```mermaid
graph TB
    subgraph CONTEXT["构建项目上下文"]
        direction TB

        LOC["定位图 (Locator Map)<br/>──────<br/>• 目标：为什么做这个项目？<br/>• 客户需求：谁在用？他们要什么？<br/>• 成功指标：怎么衡量成功？<br/>• 赞助人：谁在付钱？想要什么？<br/>• 固定约束：预算、截止日期、人力<br/>• 风险：什么可能出错？"]

        TOPO["地形图 (Topographical Map)<br/>──────<br/>• 历史：项目从哪来？之前失败过吗？<br/>• 团队：谁参与？谁是leader？<br/>  谁是真正的影响者？<br/>• 政治：谁支持谁反对？<br/>• 决策方式：怎么做决策？"]

        TREASURE["藏宝图 (Treasure Map)<br/>──────<br/>• 我们要去哪？<br/>• 路上有哪些里程碑？<br/>• 终点是什么样的？"]
    end

    style CONTEXT fill:#f8f9fa,stroke:#333
```

#### 关于客户需求的关键故事

> 作者讲了一个故事：她加入新基础设施团队的第一周，一个同事描述他们正在为另一个团队升级系统以提供新功能。她问"他们为什么需要这个功能？"同事说"也许需要吧，但我们不确定，也没有办法知道。"——**这两个团队就在同一栋楼的同一层。**
>
> 即使是最内部的项目，你也有"客户"。如果没有产品经理帮你搞清需求，你就得自己去做。不要想当然。

#### 关于成功指标

```mermaid
graph TB
    METRICS["如何定义成功指标"]

    METRICS --> M1["代码行数 ≠ 成功<br/>──────<br/>写了多少代码不说明<br/>任何问题是否被解决<br/>有时候成功 = 删代码"]

    METRICS --> M2["找客观指标<br/>──────<br/>更多收入？更少宕机？<br/>流程耗时减半？<br/>能设一个前后对比的基准吗？"]

    METRICS --> M3["Sarah Wells 的例子<br/>──────<br/>迁移150+微服务到K8s<br/>用两个指标衡量：<br/>1. 维护集群的时间<br/>2. Slack上团队成员抱怨<br/>功能不正常的消息数"]

    METRICS --> M4["如果你发起了这个项目<br/>──────<br/>要更严格！<br/>用最大的怀疑对待自己的想法<br/>设定快速、可度量的目标<br/>好知道趋势是否正确<br/>信誉和社会资本可以涨也可以跌"]

    style METRICS fill:#1a73e8,color:#fff
```

---

### 搭建项目结构

```mermaid
graph TB
    STRUCTURE["项目结构的四个支柱"]

    STRUCTURE --> S1["定义角色"]
    STRUCTURE --> S2["达成范围共识"]
    STRUCTURE --> S3["约定后勤事项"]
    STRUCTURE --> S4["开启动会"]

    style STRUCTURE fill:#f8f9fa,stroke:#333
```

#### 定义角色：谁做什么？

在 Staff 工程师级别，多个领导角色之间的边界很模糊。一个资深工程师、一个工程经理、一个TPM——他们都应该"做房间里的成年人"。谁负责什么？

**作者的推荐方法：职责分配表**

| 职责 | 负责人 |
|---|---|
| 理解客户需求、提供初始需求 | 产品经理 |
| 提供产品成功 KPI | 产品经理 |
| 设定时间线 | TPM |
| 设定范围和里程碑 | 产品经理 + 工程经理 |
| 招募新团队成员 | 工程经理 |
| 监控团队健康 | 工程经理 |
| 技术指导和辅导 | 技术领导 (你) |
| 高层架构设计 | 技术领导（团队支持） |
| 编码 | 工程团队（技术领导支持） |
| 沟通项目状态 | TPM |
| 技术最终决策 | 技术领导 |
| 用户可见行为最终决策 | 产品经理 |

**更正式的工具：RACI 矩阵**

```mermaid
graph TB
    RACI["RACI 矩阵"]

    RACI --> R["Responsible 负责<br/>实际做工作的人"]
    RACI --> A["Accountable 问责<br/>对交付负最终责任<br/>每项任务只有一个"]
    RACI --> C["Consulted 咨询<br/>被征求意见的人"]
    RACI --> I["Informed 通知<br/>被通知进展的人"]

    BOTTOM["Google 一位 Staff 工程师的评价：<br/>'RACI 帮我们打破了两个坏模式：<br/>因为不知道谁是决策者就无限讨论，<br/>以及因为没有决策流程就反复重审每个决策。<br/>它没有完全解决问题，<br/>但提供了不引起争议的结构。'"]

    R & A & C & I --> BOTTOM

    style RACI fill:#1a73e8,color:#fff
    style BOTTOM fill:#e6f4ea,stroke:#34a853
```

> **关键原则：如果你是项目领导，你隐含地负责所有没人做的事。** 没人跟踪用户需求？是你。没人做项目管理？是你。这可能累积成很大的负担。

#### 达成范围共识

```mermaid
graph TB
    SCOPE["达成范围共识"]

    SCOPE --> SC1["项目管理铁三角<br/>──────<br/>时间、预算、范围<br/>'快、便宜、好——选两个'<br/>人更少 = 能做的更少<br/>看似显而易见但很容易忘"]

    SCOPE --> SC2["增量交付<br/>──────<br/>不要一次性交付<br/>每个里程碑都应该是<br/>可用的、可演示的<br/>给用户反馈的机会<br/>——Jackie Benowitz:<br/>'每个里程碑都是beta测试'"]

    SCOPE --> SC3["工作流拆分<br/>──────<br/>大项目拆成 workstreams<br/>每个有自己的里程碑<br/>可以独立讨论进度<br/>也可以拆成 phases<br/>完成一大块后重新定向"]

    SCOPE --> SC4["里程碑的动力<br/>──────<br/>保持一个里程碑在视线内<br/>——可达的目标有动力<br/>人不会对'以后某个时候'<br/>产生紧迫感<br/>截止日期创造行动"]

    style SCOPE fill:#fef7e0,stroke:#f9ab00
```

#### 时间估算

```mermaid
graph TB
    TIME["时间估算"]

    TIME --> T1["几乎没人擅长这个<br/>──────<br/>最常见建议1：<br/>拆成最小任务<br/>最常见建议2：<br/>乘以3<br/>都不太令人满意"]

    TIME --> T2["更好的方法<br/>──────<br/>《程序员修炼之道》：<br/>'确定时间表的唯一方式<br/>是在那个项目上获取经验'<br/>先交付小切片<br/>然后根据实际速度更新估算"]

    TIME --> T3["考虑依赖团队<br/>──────<br/>有些团队全职投入你的项目<br/>有些只是众多优先级之一<br/>尽早沟通<br/>越晚告诉他们需求<br/>越不可能得到配合<br/>打断他们 = 打乱他们的估算"]

    TIME --> T4["练习估算<br/>──────<br/>像其他技能一样<br/>需要刻意练习<br/>记录估算和实际<br/>看自己偏差在哪"]

    style TIME fill:#f3e8fd,stroke:#9334e6
```

#### 约定后勤事项

```mermaid
graph TB
    LOGISTICS["后勤事项清单"]

    LOGISTICS --> L1["会议节奏<br/>每天standup？每周leads会？<br/>定期回顾？演示？"]

    LOGISTICS --> L2["非正式沟通<br/>Slack社交频道、破冰活动<br/>meme频道都有用<br/>远程团队尤其需要"]

    LOGISTICS --> L3["状态共享<br/>赞助人怎么知道进展？<br/>其他利益相关方？<br/>定期邮件更新？仪表盘？"]

    LOGISTICS --> L4["文档之家<br/>创建一个项目主页<br/>链接到一切<br/>好记的URL<br/>——混乱宇宙中的固定点"]

    LOGISTICS --> L5["开发实践<br/>语言？部署方式？<br/>代码审查标准？<br/>测试覆盖要求？<br/>feature flag？"]

    style LOGISTICS fill:#e8f0fe,stroke:#1a73e8
```

#### 启动会（Kickoff Meeting）

```mermaid
graph TB
    KICKOFF["启动会议程"]

    KICKOFF --> K1["所有人是谁"]
    KICKOFF --> K2["项目目标是什么"]
    KICKOFF --> K3["目前为止发生了什么"]
    KICKOFF --> K4["搭建了什么结构"]
    KICKOFF --> K5["接下来要做什么"]
    KICKOFF --> K6["你希望大家做什么——<br/>⚠️ 要明确说！<br/>模糊 = 你在给所有人<br/>制造额外工作<br/>让他们去猜你的意思"]
    KICKOFF --> K7["怎么提问和获取更多信息"]

    style KICKOFF fill:#34a853,color:#fff
```

---

## 二、驾驶项目

Kripa Krishnan (Google Cloud VP) 的核心比喻：

> **"驾驶不是踩油门然后直线走。"** 驾驶是主动的、审慎的角色——选择路线、做决策、对路上的危险做出反应。你在驾驶座上，负责把所有人安全送到目的地。

```mermaid
graph LR
    subgraph DRIVING["驾驶项目的四个阶段"]
        direction LR
        D1["探索<br/>Exploring"] --> D2["厘清<br/>Clarifying"] --> D3["设计<br/>Designing"] --> D4["编码 + 沟通<br/>Coding + Communicating"]
    end

    style DRIVING fill:#f8f9fa,stroke:#333
```

---

### 探索阶段

```mermaid
graph TB
    EXPLORE["探索阶段"]

    EXPLORE --> E1["怀疑预制方案<br/>──────<br/>如果新项目开始就有<br/>详细设计文档和实现方案<br/>——要警惕<br/>如果问题真那么直接<br/>需要 Staff 工程师吗？<br/>你需要先探索"]

    EXPLORE --> E2["理解项目的重要方面<br/>──────<br/>越大的项目<br/>越可能每个团队<br/>有不同的心智模型<br/>他们对目标、范围、词汇<br/>可能完全不同<br/>——统一理解是你的工作"]

    EXPLORE --> E3["评估可能的方案<br/>──────<br/>先搞清要解决什么<br/>再决定怎么做<br/>对已有的解决方案保持开放<br/>——可能不够完美<br/>但可能更实际<br/>从历史中学习"]

    style EXPLORE fill:#e8f0fe,stroke:#1a73e8
```

### 厘清阶段：把复杂变简单

```mermaid
graph TB
    CLARIFY["厘清：给所有人建立心智模型"]

    CLARIFY --> CL1["心智模型<br/>──────<br/>类比：<br/>'想象这部分像UNIX进程'<br/>不需要完美<br/>需要'足够近'<br/>让人能挂钩新知识<br/><br/>Alain de Botton:<br/>'没有连接链的事实<br/>就像散落的项链珠子<br/>——无用且转瞬即逝'"]

    CLARIFY --> CL2["命名<br/>──────<br/>两个人用同一个词<br/>可以指完全不同的东西<br/>Eric Evans《领域驱动设计》：<br/>'统一语言 (Ubiquitous Language)'<br/>在你的领域建立共享词汇<br/>如果和多个群体沟通<br/>提供词汇表"]

    CLARIFY --> CL3["画图<br/>──────<br/>没有比图片<br/>更容易帮人理解的工具<br/>before/after 对比<br/>比一篇文章更清晰<br/>⚠️ 注意颜色含义<br/>（绿色=好、红色=停？）<br/>圆柱体=数据库？"]

    style CLARIFY fill:#fef7e0,stroke:#f9ab00
```

### 设计阶段：写下来

#### 为什么要写设计文档？

```mermaid
graph TB
    WHY_WRITE["为什么要写设计文档"]

    WHY_WRITE --> W1["'写下来的设计是<br/>极其廉价的迭代'<br/>—— Cian Synnott<br/>──────<br/>在代码之前发现问题<br/>比之后便宜得多"]

    WHY_WRITE --> W2["沉默不等于同意<br/>──────<br/>你口头说了方案<br/>没人反对<br/>不意味着他们同意<br/>也可能他们没理解<br/>写下来才能确认"]

    WHY_WRITE --> W3["WRONG IS BETTER<br/>THAN VAGUE<br/>──────<br/>写一个可能错误的方案<br/>比写一个含糊的方案好<br/>错了别人会纠正你<br/>你会学到东西<br/>含糊了没人能反驳<br/>也没人能帮你改进"]

    style WHY_WRITE fill:#34a853,color:#fff
```

#### RFC 文档的核心结构

```mermaid
graph TB
    subgraph RFC["RFC 文档核心结构"]
        direction TB

        subgraph MUST["必须包含的部分"]
            RFC1["Context 上下文<br/>标题、作者、日期、状态<br/>让两年后的人能判断<br/>是否值得读"]
            RFC2["Goals 目标<br/>为什么做这个？<br/>要解决什么问题？<br/>⚠️ 目标≠实现方案<br/>'创建serverless API'是实现<br/>'让用户能实时查询'是目标"]
            RFC3["Design 设计<br/>怎么达成目标<br/>几段话或几十页<br/>关键是让读者能评估<br/>方案是否可行"]
            RFC4["Security/Privacy<br/>安全/隐私/合规<br/>即使你认为不相关<br/>也要写下为什么不相关"]
            RFC5["Alternatives Considered<br/>考虑过的替代方案<br/>为什么不用更简单的？<br/>为什么不用现有的？<br/>⚠️ 如果跳过这个<br/>说明你可能没想清楚"]
        end

        subgraph NICE["建议包含的部分"]
            RFC6["Background 背景<br/>读者需要什么信息？<br/>可以包含词汇表"]
            RFC7["Trade-offs 权衡<br/>设计的缺点是什么？<br/>你做了什么有意的妥协？"]
            RFC8["Risks 风险<br/>什么可能出错？<br/>最坏情况是什么？"]
            RFC9["Dependencies 依赖<br/>需要其他团队提供什么？<br/>他们知道你要来了吗？"]
            RFC10["Operations 运维<br/>谁来运行这个系统？<br/>怎么监控？谁on-call？"]
        end
    end

    style RFC fill:#f8f9fa,stroke:#333
    style MUST fill:#e8f0fe,stroke:#1a73e8
    style NICE fill:#fef7e0,stroke:#f9ab00
```

#### 写作两条黄金原则

```mermaid
graph LR
    subgraph TIPS["设计文档写作的两条原则"]
        direction TB
        TIP1["用主动语态<br/>──────<br/>❌ '数据将被加密传输'<br/>  （谁加密？怎么加密？）<br/>✅ '客户端在传输前<br/>  加密数据'<br/><br/>检测被动语态的方法：<br/>'如果动词后面可以加<br/>by zombies<br/>那就是被动语态'"]

        TIP2["避免模糊代词<br/>──────<br/>❌ 'A提案替代B方案<br/>  B提供原始功能<br/>  Team需要this<br/>  所以要讨论需求'<br/>  （this指什么？）<br/>✅ 直接重复名词<br/>  即使刚提过<br/>  清晰胜于简洁"]
    end

    style TIPS fill:#e6f4ea,stroke:#34a853
```

### 技术设计的常见陷阱

```mermaid
graph TB
    PITFALLS["技术设计的常见陷阱"]

    PITFALLS --> PF1["'这是全新问题'<br/>但其实不是<br/>──────<br/>你的问题几乎肯定不是全新的<br/>去研究先例和现有方案<br/>不要错过向他人学习的机会"]

    PITFALLS --> PF2["'这看起来很简单'<br/>──────<br/>有些项目诱人地看起来简单<br/>但水下的冰山巨大<br/>'构建一个会计系统'<br/>'构建一个工资系统'<br/>——如果看起来简单<br/>说明你还不理解它"]

    PITFALLS --> PF3["为现在建还是为未来建？<br/>──────<br/>设计5倍用量？也许合理<br/>但小心过度工程化<br/>'以后可能需要'<br/>不是好的理由<br/>写下为什么值得额外投入"]

    PITFALLS --> PF4["'难的部分以后再说'<br/>──────<br/>常见于迁移项目：<br/>花一个季度建系统<br/>让简单用例跑通<br/>然后发现困难用例<br/>根本跑不通<br/>→ 先验证困难的部分！"]

    PITFALLS --> PF5["解决小问题时<br/>让大问题更难<br/>──────<br/>hack式解决方案<br/>产生对现有行为的隐性依赖<br/>让以后的根本方案更难实施"]

    PITFALLS --> PF6["'它不是真的重写'<br/>但其实是<br/>──────<br/>'只是重构''只是迁到云上'<br/>——但除非代码已经很模块化<br/>否则你会重写远超预期的量<br/>如果是重写就承认它"]

    PITFALLS --> PF7["可运维吗？<br/>──────<br/>下午3点你能理解的东西<br/>凌晨3点你理解不了<br/>让系统可观测、可调试<br/>让流程尽量无聊和自文档化"]

    PITFALLS --> PF8["Bikeshedding<br/>自行车棚讨论<br/>──────<br/>Parkinson 琐碎定律：<br/>越简单的决策讨论越多<br/>核电站委员会花大量时间<br/>讨论自行车棚的材料<br/>——你也会在琐碎可逆的事上<br/>写长段反馈<br/>却不碰真正困难的决策"]

    style PITFALLS fill:#fce8e6,stroke:#ea4335
```

### 编码：Staff 工程师该不该写代码？

```mermaid
graph TB
    CODING["Staff 工程师在项目中的编码"]

    CODING --> CD1["要不要写代码？<br/>──────<br/>取决于项目大小、团队大小、<br/>你的偏好<br/>小团队：你可能深入每行代码<br/>大项目多团队：<br/>你可能审阅大量代码<br/>但自己写很少"]

    CODING --> CD2["Joy Ebertz 的观点<br/>──────<br/>'写代码很少是你时间<br/>最高杠杆的用法<br/>我今天写的代码<br/>一个更初级的人也能写'<br/><br/>但同时：<br/>'写代码给你难以替代的<br/>深度理解<br/>一天写代码让你<br/>对工作的其余部分更投入'"]

    CODING --> CD3["如果你写代码<br/>──────<br/>⚠️ 不要接关键路径上的任务<br/>你的时间不可预测<br/>会议更多<br/>→ 你完成代码会更慢<br/>→ 成为瓶颈<br/><br/>选择非关键路径的、<br/>有杠杆效应的代码<br/>如建立模式让他人复用"]

    CODING --> CD4["做榜样而非瓶颈<br/>──────<br/>Katrina Owen (GitHub Staff)：<br/>她建立了API分页测试的标准方式<br/>替换所有现有测试<br/>→ 以后所有人都会照着写<br/><br/>Ross Donaldson：<br/>'我像侦察兵和制图师<br/>发现问题、带回信息<br/>搭个粗桥让团队改进<br/>优先保护他们的ownership<br/>而非满足我的代码美学'"]

    CODING --> CD5["代码审查的注意<br/>──────<br/>Staff 审查初级工程师代码<br/>可能让对方很紧张<br/>你要被视为资源<br/>而非批评每个决策的人<br/><br/>有时候'足够好'就够了<br/>不要替别人设模式<br/>不要做所有code review<br/>否则你成单点故障<br/>团队学不到东西"]

    style CODING fill:#f3e8fd,stroke:#9334e6
```

### 沟通

```mermaid
graph TB
    COMMS["沟通的两个层面"]

    COMMS --> C1["团队内部沟通<br/>──────<br/>让团队成员经常交流<br/>建立关系<br/>跨团队的人参加彼此的standup<br/>目标：舒适的熟悉感<br/>让提问和表达不同意<br/>都不觉得尴尬<br/><br/>不认识的工程师之间<br/>说'我不明白那是什么意思'<br/>会很不自在<br/>→ 误解不被发现"]

    COMMS --> C2["对外状态共享<br/>──────<br/>让利益相关方<br/>容易找到项目状态<br/>一对一、群邮件、仪表盘<br/><br/>⚠️ 用影响力来报告<br/>不要报告'站了3个微服务'<br/>报告'用户现在能做X了'<br/><br/>⚠️ 先讲结论<br/>不要假设他们会细读<br/>把关键信息放在最前面<br/><br/>⚠️ 诚实报告<br/>项目遇困不要报绿灯<br/>避免'西瓜项目'：<br/>外面绿色里面红色<br/>卡住了就求助"]

    style COMMS fill:#1a73e8,color:#fff
```

---

## 三、导航：遇到障碍时

```mermaid
graph TB
    NAV["遇到障碍时"]

    NAV --> N1["这是不可避免的<br/>──────<br/>技术不合适、法务否决、<br/>业务方向变了、关键人离职<br/>假设会出问题<br/>只是不知道什么时候<br/>→ 更灵活地应对"]

    NAV --> N2["你不能说<br/>'项目被阻塞了所以没办法'<br/>──────<br/>你负责重新规划路线<br/>或向上求助<br/>或告诉利益相关方<br/>目标不可达了"]

    NAV --> N3["避免西瓜项目<br/>──────<br/>如果一个关键问题<br/>无法解决<br/>而项目其他部分都是绿色<br/>→ 项目状态不是绿色<br/>不要隐瞒困难求助"]

    NAV --> N4["你不是一个人<br/>──────<br/>你的管理者想让你成功<br/>你的 Director 想让组织成功<br/>如果你不告诉他们<br/>你需要帮助<br/>他们就更难做好自己的工作<br/>求助不是失败<br/>不求助才是"]

    style NAV fill:#fce8e6,stroke:#ea4335
```

---

## 本章总结

```mermaid
graph TB
    subgraph RECAP["第五章核心要点"]
        direction TB
        R1["Staff 工程师能把不可能的问题<br/>变成可管理的问题"]
        R2["感到不知所措很正常<br/>项目困难正是需要你的原因"]
        R3["搭建结构来减少模糊<br/>让上下文共享变容易"]
        R4["明确成功的定义<br/>以及怎么衡量它"]
        R5["领导项目意味着主动驾驶<br/>不是被动等事情发生"]
        R6["通过建立关系和信任<br/>来铺平道路"]
        R7["写下来。清晰且有观点<br/>错的会被纠正<br/>含糊的会一直含糊"]
        R8["会有权衡取舍<br/>明确你在优化什么"]
        R9["面向受众沟通<br/>频繁且诚实"]
        R10["预期问题会出现<br/>计划应假设<br/>方向会变、人会走、依赖会断"]
    end

    style RECAP fill:#f8f9fa,stroke:#333
```

> **一句话总结：领导大项目的核心不是技术天才，而是在模糊和混乱中保持清醒——构建上下文、搭建结构、驾驶方向、写下设计、诚实沟通、遇到障碍时主动导航而非被动等待。困难就是重点——如果项目不难，就不需要你了。**

---

**上一章**：[[ch4_有限的时间]] —— 如何管理你的时间和资源
**下一章**：[[ch6_为什么停滞了]] —— 项目停滞的原因和重启方法
