# 第六章：为什么停滞了？

> **原书标题**：Why Have We Stopped?
> **所属**：Part II - Execution（执行力）
> **核心问题**：当项目停滞不前时，作为 Staff 工程师如何诊断原因、解除阻塞、应对迷失方向、以及优雅地结束项目？

---

## 本章结构

```mermaid
flowchart TB
    A["项目停下来了<br/>── 你的职责是搞清为什么<br/>并重新启动"] --> B["被卡住了<br/>── 外部阻塞"]
    B --> C["迷路了<br/>── 不知道方向"]
    C --> D["到达了……某个地方<br/>── 假完成的三种形态"]
    D --> E["项目走到了尽头<br/>── 四种结局"]

    style A fill:#fef7e0,stroke:#f9ab00
    style B fill:#fce8e6,stroke:#ea4335
    style C fill:#e8f0fe,stroke:#1a73e8
    style D fill:#f3e8fd,stroke:#9334e6
    style E fill:#e6f4ea,stroke:#34a853
```

---

## 核心前提：你不只是你自己项目的领导者

```mermaid
graph TB
    PREMISE["你作为组织中的领导者"]

    PREMISE --> P1["你能帮助你没有领导的项目<br/>──────<br/>Will Larson:<br/>'惊人数量的项目只差<br/>一个小改变就能成功<br/>一个快速修改就能解锁<br/>一次对话就能达成共识'<br/><br/>用你的组织特权、关系和经验<br/>最小的努力投入<br/>就能改变项目的结局"]

    PREMISE --> P2["但要有纪律<br/>──────<br/>不要让支线任务和帮忙<br/>吃掉你自己的主项目时间<br/>（参见第四章时间图）<br/>选择性地介入<br/>并有退出计划"]

    style PREMISE fill:#1a73e8,color:#fff
```

### 解除阻塞的四种通用技术

全章反复使用这四种方法来应对各种类型的阻塞：

```mermaid
graph TB
    FOUR["四种解除阻塞的技术"]

    FOUR --> T1["理解并解释<br/>Understand and Explain<br/>──────<br/>诊断到底发生了什么<br/>确保所有人有相同理解<br/>写下来——<br/>综合信息本身就能<br/>产生新洞察"]

    FOUR --> T2["降低工作难度<br/>Make the Work Easier<br/>──────<br/>减少你需要别人做的量<br/>把请求结构化<br/>让人容易说'是'"]

    FOUR --> T3["获取组织支持<br/>Get Organizational Support<br/>──────<br/>展示这是组织目标<br/>必要时向上升级<br/>⚠️ 升级≠告状<br/>= 和有权力的人<br/>一起解决问题"]

    FOUR --> T4["制定替代方案<br/>Make Alternative Plans<br/>──────<br/>阻塞不会消失时<br/>调整范围、换方向<br/>或接受延期"]

    style FOUR fill:#e6f4ea,stroke:#34a853
```

---

## 一、被卡住了（You're Stuck in Traffic）

### 被另一个团队阻塞

```mermaid
graph TB
    BLOCKED_TEAM["被另一个团队阻塞"]

    BLOCKED_TEAM --> BT1["为什么他们不做？"]

    BT1 --> R1["误解 (Misunderstanding)<br/>──────<br/>他们根本不知道有截止日期<br/>或对要做的事理解不同<br/>信息在传递中丢失了"]

    BT1 --> R2["不幸 (Misadventure)<br/>──────<br/>有人离职了、生病了<br/>团队人手不够<br/>或被自己的依赖阻塞<br/>他们可能真的做不到"]

    BT1 --> R3["优先级不对齐 (Misalignment)<br/>──────<br/>你的项目是他们的第三优先级<br/>他们在做自己的最高优先级<br/>——完全合理<br/>但你的项目就是推不动"]

    style BLOCKED_TEAM fill:#fce8e6,stroke:#ea4335
```

#### 优先级不对齐的典型场景

```mermaid
graph TB
    subgraph ORG["组织优先级"]
        direction TB
        O1["Project A: Team 1"]
        O2["Project B: Team 1"]
        O3["Project C: Team 1 + Team 2"]
        O4["Project D: Team 2"]
    end

    subgraph T1["Team 1 的优先级排序"]
        direction TB
        T1A["1. Project A"]
        T1B["2. Project B"]
        T1C["3. Project C ← 你的项目"]
    end

    subgraph T2["Team 2 的优先级排序"]
        direction TB
        T2A["1. Project C ← 你的项目"]
        T2B["2. Project D"]
    end

    RESULT["结果：Team 2 全力投入你的项目<br/>但 Team 1 在做其他的事<br/>→ Team 2 一直在等 Team 1<br/>你的项目整体卡住"]

    T1 & T2 --> RESULT

    style RESULT fill:#fce8e6,stroke:#ea4335
```

#### 应对被团队阻塞

```mermaid
graph TB
    NAV_TEAM["应对策略"]

    NAV_TEAM --> N1["理解并解释<br/>──────<br/>主动去问<br/>DM/邮件不回就约会议<br/>用你在ch2建立的桥梁<br/>解释为什么重要<br/>你希望他们什么时候做什么<br/>给他们机会说是否现实"]

    NAV_TEAM --> N2["降低工作难度<br/>──────<br/>只要求你绝对必须的特性<br/>如果他们被自己的依赖卡住<br/>帮他们解除阻塞！<br/>解决他们的问题<br/>→ 他们能解决你的<br/><br/>可以提出帮他们写代码<br/>然后让他们审查<br/>⚠️ 他们可能不接受"]

    NAV_TEAM --> N3["获取组织支持<br/>──────<br/>如果对方确实在做更高优先级的事<br/>确认他们是对的<br/>如果优先级不清楚<br/>让管理层裁决<br/>⚠️ 升级不是告状<br/>是和有权力的人<br/>礼貌地解决问题<br/>保持建设性"]

    NAV_TEAM --> N4["替代方案<br/>──────<br/>调整范围<br/>换方向<br/>延期发布<br/>告诉利益相关方情况"]

    style NAV_TEAM fill:#e8f0fe,stroke:#1a73e8
```

### 被一个"该死的按钮点击"阻塞

当你等一个只需要10分钟的审批、配置或代码审查——但它就是不发生。

```mermaid
graph TB
    BUTTON["为什么那个按钮没被点？"]

    BUTTON --> BU1["他们也有一百个'按钮'要点<br/>你只是其中之一"]

    BUTTON --> BU2["他们可能在改进流程<br/>短期变得更慢了"]

    BUTTON --> BU3["审批 = 承担责任<br/>安全团队、合规团队<br/>不会随便点批准<br/>他们批了就要对后果负责"]

    BUTTON --> BU4["作者自己的故事<br/>──────<br/>她曾在负载均衡团队<br/>文档说需要一周通知<br/>但实际操作只要半小时<br/>别的团队总是前一天才来<br/>→ 团队内部的口头禅变成：<br/>'你的计划不周不等于我的紧急'<br/><br/>回头看，她说两边都有道理<br/>——视角 (perspective) 不同"]

    style BUTTON fill:#fef7e0,stroke:#f9ab00
```

**应对策略：**
- 礼貌地道歉并请求加急——道歉比吼叫有效得多
- 感谢帮过你的人（peer bonus、送茶、在发布邮件里点名感谢）
- 结构化你的请求——让它成为最容易处理的那种请求（参见原书 Figure 6-3：信息齐全、不需要思考的工单）
- 有人脉关系帮助——有时候认识一个人就够了

### 被一个人阻塞

```mermaid
graph TB
    PERSON["被单个人阻塞"]

    PERSON --> PS1["表面原因可能不是真实原因<br/>──────<br/>同事说'下周就好'<br/>但一直没好<br/>可能不是偷懒<br/>而是：被吓住了、<br/>不知道从何开始、<br/>个人生活有压力、<br/>或者领导暗示<br/>你的项目不重要"]

    PERSON --> PS2["帮助不等于接管<br/>──────<br/>结对编程通过第一步<br/>帮他们拆解任务<br/>创建里程碑<br/>⚠️ 但不要替他们做<br/>他们不学会开车<br/>如果你抢方向盘"]

    PERSON --> PS3["沟通技巧<br/>──────<br/>'Three Bullets and<br/>a Call to Action'<br/>（Debugging Teams 一书）<br/>最多三个要点 + 一个行动请求<br/>再多就被忽略<br/>或者只回复你<br/>最不在乎的那条"]

    PERSON --> PS4["最后手段：升级<br/>──────<br/>如果他们就是不做<br/>这是人员管理问题<br/>和他们的管理者谈<br/>升级不是告状<br/>是寻求帮助"]

    style PERSON fill:#fce8e6,stroke:#ea4335
```

### 被未分配的工作阻塞

```mermaid
graph TB
    UNASSIGNED["没有人负责的工作"]

    UNASSIGNED --> UA1["这是'板块构造'问题<br/>──────<br/>每个团队有明确边界<br/>强边界保持聚焦<br/>但现在有一块关键的基础工作<br/>不属于任何人<br/>所有人都有意见<br/>但没人写代码"]

    UNASSIGNED --> UA2["藏宝图上的不可逾越山脊<br/>──────<br/>不管你选哪条技术路径<br/>都要翻过这座山<br/>——缺乏人员和团队<br/>纯粹在技术方案上争论<br/>是在浪费时间"]

    UNASSIGNED --> UA3["你的最高价值工作<br/>──────<br/>写一份 rollup<br/>——综合所有信息<br/>画出明确的结论<br/>（见侧栏：Denise Yu 的<br/>'The Art of the Rollup'）<br/><br/>志愿加入或指导<br/>为赞助人和人员支持做倡导<br/>⚠️ 如果人员承诺<br/>永远是'下季度'<br/>→ 这不是组织的优先级<br/>→ 推迟项目"]

    style UNASSIGNED fill:#f3e8fd,stroke:#9334e6
```

#### Rollup 技术（综合信息）

> Denise Yu（GitHub 管理者）描述了 **"the art of the rollup"**：把所有信息汇总到一个地方，"创造清晰、减少混乱"。
>
> 汇总事实本身就是构建知识的好方法。但写下来还可能产出新信息——Alex 说 "新库会给这个端点提供认证"，后来 Meena 说 "我们要到 Q3 才能升级到新版库"。你可以写下推论：**"我们最早 Q3 才会有认证"**。这个结论对有全部上下文的人来说显而易见，但如果没人综合过，可能没人意识到。

### 被大量人群阻塞（迁移场景）

```mermaid
graph TB
    CROWD["迁移项目：所有人都需要配合"]

    CROWD --> CW1["每个人都有自己的故事<br/>──────<br/>有人支持但没时间<br/>有人反对（偏好旧系统）<br/>有人不在乎但疲于<br/>无尽的升级和替换<br/>——所有人都是合理的"]

    CROWD --> CW2["半完成的迁移最糟<br/>──────<br/>团队要同时支持新旧系统<br/>新用户不知道用哪个<br/>每个人都更痛苦"]

    style CROWD fill:#fce8e6,stroke:#ea4335
```

#### 推进迁移的实战技巧

```mermaid
graph TB
    MIGRATION["推进迁移的技巧"]

    MIGRATION --> MG1["让推进方承担更多工作<br/>──────<br/>能自动化的自动化<br/>具体告诉每个团队<br/>要编辑哪些文件<br/>新系统必须真的能用<br/>别折腾用户"]

    MIGRATION --> MG2["让新方式成为默认<br/>──────<br/>更新所有指向旧方式的<br/>文档、代码和流程<br/>考虑让旧方式<br/>需要特殊配置才能用<br/>如 `i_know_this_is_unsupported`"]

    MIGRATION --> MG3["展示进度<br/>──────<br/>作者做过一个项目<br/>需要数百个团队改配置<br/>分享'已完成 vs 剩余'的图表<br/>人们看到图表要归零<br/>就更有动力完成自己的部分"]

    MIGRATION --> MG4["和不肯动的团队结对<br/>──────<br/>有些团队真的太忙<br/>有些有你自动化没覆盖的用例<br/>有些组件已没人维护<br/>→ 帮他们做或和他们结对"]

    MIGRATION --> MG5["最终手段（需要组织支持）<br/>──────<br/>撤销对旧方式的支持<br/>引入人为摩擦（变慢）<br/>甚至定期关闭<br/>⚠️ 必须有强组织支持<br/>不能伤害用户<br/>要理智<br/><br/>最后的最后：<br/>让不肯迁移的团队<br/>自己维护那个旧组件<br/>'最后关灯的人负责'"]

    style MIGRATION fill:#e6f4ea,stroke:#34a853
```

---

## 二、迷路了（You're Lost）

不是被外部事物阻塞，而是**你自己不知道该往哪走**。三种迷路形态：

```mermaid
graph TB
    LOST["三种迷路形态"]

    LOST --> L1["不知道去哪<br/>──────<br/>方向不明确<br/>太多利益相关方<br/>太多意见<br/>分析瘫痪"]

    LOST --> L2["不知道怎么去<br/>──────<br/>目标清晰<br/>但不知道下一步<br/>项目太难或太大<br/>可能有冒名顶替综合症"]

    LOST --> L3["不知道自己的处境<br/>──────<br/>组织支持是否还在？<br/>赞助人是否还关心？<br/>项目是否还存在？<br/>——最可怕的一种"]

    style LOST fill:#e8f0fe,stroke:#1a73e8
```

### 不知道去哪：如何选择方向

```mermaid
graph TB
    NO_DEST["如何选择方向"]

    NO_DEST --> ND1["明确角色<br/>──────<br/>你不能只是房间里的一个声音<br/>明确你将最终做决策<br/>（但会先听取所有人意见）<br/>如果你没这个权力<br/>让赞助人授予你"]

    NO_DEST --> ND2["选择一个策略<br/>──────<br/>在对齐目标之前<br/>禁止讨论实现细节<br/>选一个小组做策略<br/>策略必然有取舍<br/>不可能让所有人满意<br/>→ 参见第三章"]

    NO_DEST --> ND3["选择一个问题<br/>──────<br/>如果没时间做完整策略<br/>选一个具体问题开始做<br/>任何刻意的方向<br/>都好过冻在原地<br/>'错的比含糊好'"]

    NO_DEST --> ND4["选择一个利益相关方<br/>──────<br/>不要解决'共享数据库很烂<br/>要重新架构'<br/>而是解决'这个团队想把<br/>数据迁出去'<br/>用'垂直切片'——<br/>先帮一个人完成一件事<br/>进展能打破僵局"]

    style NO_DEST fill:#fef7e0,stroke:#f9ab00
```

### 不知道怎么去：在困难中找路

```mermaid
graph TB
    NO_PATH["找路的八种方法"]

    NO_PATH --> NP1["清晰表述问题<br/>写出来或大声说出来<br/>注意哪里不够精确"]

    NO_PATH --> NP2["重新审视假设<br/>你是否卡在一个预设方案里？<br/>有些trade-off是可接受的吗？<br/>是否因为'太简单'而排除方案？"]

    NO_PATH --> NP3["给它时间<br/>睡眠和假期能做到<br/>清醒状态做不到的事<br/>离开几天再回来<br/>几乎总是有更好的想法"]

    NO_PATH --> NP4["增加思考容量<br/>在会议间隙思考 → 想法受限<br/>给自己预留大块时间<br/>好的睡眠、食物、水"]

    NO_PATH --> NP5["查找先例<br/>你可能不是第一个遇到这个问题的人<br/>看内部和外部<br/>航空、土木、医学<br/>都有现成的解决方案"]

    NO_PATH --> NP6["向他人学习<br/>和赞助人、利益相关方、<br/>外部社区交流<br/>别人的经验 = 时间节省"]

    NO_PATH --> NP7["换个角度<br/>技术问题试组织方案<br/>组织问题试技术方案<br/>如果外包会怎样？<br/>如果你不在了谁来做？"]

    NO_PATH --> NP8["从小处开始<br/>先解决一个小部分<br/>哪怕是粗糙的方案<br/>让进展带来信心<br/>然后迭代"]

    style NO_PATH fill:#e6f4ea,stroke:#34a853
```

### 不知道自己的处境：你的项目还存在吗？

```mermaid
graph TB
    NO_STAND["不确定项目是否还有支持"]

    NO_STAND --> NS1["信号<br/>──────<br/>赞助人不再check in<br/>管理者看起来不感兴趣<br/>公司重要项目列表<br/>没有你的项目<br/>同事用'如果'而非'什么时候'<br/>来谈你的项目"]

    NO_STAND --> NS2["可能发生了什么<br/>──────<br/>新VP有不同想法<br/>公司方向变了<br/>有人用更大范围<br/>接管了你的项目<br/>或者只是：<br/>你做得太好了<br/>没人觉得需要看"]

    NO_STAND --> NS3["怎么做"]

    NS3 --> A1["确认组织支持<br/>准备好可能不喜欢答案<br/>直接问赞助人/管理者"]
    NS3 --> A2["明确角色<br/>如果你是lead但不敢说<br/>→ 正式化它<br/>'非官方lead'<br/>= 失败的邀请"]
    NS3 --> A3["要求你需要的东西<br/>在全员会上提到项目<br/>列入组织目标<br/>不问不会得到"]
    NS3 --> A4["充能<br/>如果士气低<br/>重新kickoff<br/>'欢迎来到Phase 2'<br/>比'继续做，这次不同了'<br/>更有动力<br/>加1-2个有热情的新人"]

    style NO_STAND fill:#fce8e6,stroke:#ea4335
```

---

## 三、到达了……某个地方（假完成的三种形态）

团队觉得项目完成了——**但问题没有真正被解决**。

```mermaid
graph TB
    FALSE_DONE["三种假完成"]

    FALSE_DONE --> FD1["代码完成了！<br/>But It's Code Complete!<br/>──────<br/>'新功能好了吗？' '好了！'<br/>'太好了怎么用？' 'Well…'<br/>还在staging、硬编码密码、<br/>PR还在审、还没部署<br/>——代码完成 ≠ 用户能用"]

    FALSE_DONE --> FD2["做完了但没人用<br/>It's Done but Nobody Is Using It<br/>──────<br/>平台团队花几个月<br/>造了精美的解决方案<br/>但用户不知道它存在<br/>= '小心豹子'项目"]

    FALSE_DONE --> FD3["建在地基不牢的地方<br/>It's Built on a Shaky Foundation<br/>──────<br/>MVP上线了，用户能用<br/>但所有人知道是hack拼的<br/>代码未测试、架构不可扩展<br/>接口不可复用<br/>→ 技术债推给了未来"]

    style FALSE_DONE fill:#f3e8fd,stroke:#9334e6
```

### "代码完成"的陷阱

```mermaid
graph TB
    CODE_COMPLETE["代码完成 ≠ 真正完成"]

    CODE_COMPLETE --> CC1["工程师的思维盲区<br/>──────<br/>我们把工作定义为'写软件'<br/>计划中只列写代码的任务<br/>但用户到达代码<br/>还需要：<br/>部署、监控、审批、<br/>文档更新、真正运行"]

    CODE_COMPLETE --> CC2["Heidi Waterhouse 的洞察<br/>──────<br/>'没人想要软件<br/>他们想抓一个宝可梦'<br/>用户不在乎代码用什么语言<br/>解决了什么算法难题<br/>他们要么能抓宝可梦<br/>要么不能<br/>不能的话软件等于不存在"]

    style CODE_COMPLETE fill:#fef7e0,stroke:#f9ab00
```

#### 确保用户能"抓到宝可梦"

```mermaid
graph TB
    POKEMON["确保真正完成"]

    POKEMON --> PK1["定义'完成'<br/>──────<br/>开始前就约定<br/>什么条件下才算完成<br/>Agile Alliance 的<br/>'Definition of Done'<br/>包含通用检查表<br/>+ 项目特定标准"]

    POKEMON --> PK2["做自己的用户<br/>──────<br/>亲自使用你造的东西<br/>= dogfooding<br/>你会发现一堆<br/>项目计划没覆盖的问题"]

    POKEMON --> PK3["庆祝着陆而非发射<br/>──────<br/>不要在部署时庆祝<br/>在用户开心使用时庆祝<br/>在迁移中：<br/>庆祝旧系统关闭<br/>而非新系统上线"]

    style POKEMON fill:#e6f4ea,stroke:#34a853
```

### "小心豹子"项目——做完了但没人用

```mermaid
graph TB
    LEOPARD["为什么没人用？"]

    LEOPARD --> LP1["典型场景<br/>──────<br/>内部平台团队造了好东西<br/>给它起了可爱的名字<br/>写了文档（也许）<br/>然后……停了<br/>用户不知道它存在<br/>就算看到了<br/>名字也看不出它是做什么的<br/>搜索常见关键词<br/>找不到这个方案"]

    LEOPARD --> LP2["《银河系漫游指南》类比<br/>──────<br/>'但计划是展示出来了的…'<br/>'展示？我得下到地下室<br/>在一个上锁的柜子里<br/>一个废弃的厕所里<br/>门上写着"小心豹子"'<br/><br/>创造者仿佛在试图<br/>藏起解决方案！"]

    style LEOPARD fill:#fce8e6,stroke:#ea4335
```

#### 怎么推销你的内部产品

```mermaid
graph TB
    SELL["推销你的解决方案"]

    SELL --> SL1["告诉人们<br/>──────<br/>不只是告诉一次<br/>要持续告诉<br/>发邮件、路演、<br/>在全员会上演示<br/>贴海报（如果在办公室）<br/>给关键客户提供白手套服务<br/>获取testimonials"]

    SELL --> SL2["让它容易被发现<br/>──────<br/>在用户可能搜索的<br/>所有地方都放链接<br/>如果有shortlink服务<br/>设置所有可能的拼写和变体<br/>用户的搜索词 ≠ 你的产品名"]

    SELL --> SL3["Michael R. Bernstein 的比喻<br/>──────<br/>'就像农民种了庄稼<br/>浇水除草<br/>然后把收获留在田里<br/>你需要把粮食送到人们面前<br/>告诉他们为什么想要它'"]

    style SELL fill:#34a853,color:#fff
```

### 建在地基不牢的地方

```mermaid
graph TB
    SHAKY["地基不牢怎么办"]

    SHAKY --> SH1["为什么会这样<br/>──────<br/>有时候快速出货是对的<br/>——竞争市场、验证想法<br/>但team走了之后<br/>那个cheap solution还在<br/>代码未测试、不可测试<br/>是别人都要绕过的<br/>架构hack"]

    SHAKY --> SH2["数据中心的教训<br/>──────<br/>'没有临时方案'<br/>如果有人不按标准<br/>从一个机架拉线到另一个<br/>那根线会一直在那<br/>直到服务器退役<br/>——所有临时hack都是如此"]

    SHAKY --> SH3["Staff 工程师能做什么"]

    SH3 --> A1["树立质量文化<br/>做那个永远问<br/>'测试呢？''怎么监控？'<br/>'文档要更新'的人"]
    SH3 --> A2["把基础工作变成用户故事<br/>不是'还技术债'<br/>而是'用户体验改善'<br/>或'为下个功能打基础'"]
    SH3 --> A3["争取工程师主导时间<br/>fix-it weeks、tech debt weeks<br/>20% time、passion project week<br/>轮值处理问题和改善"]

    style SHAKY fill:#f3e8fd,stroke:#9334e6
```

---

## 四、项目走到了尽头

四种项目结局：

```mermaid
graph TB
    ENDINGS["项目的四种结局"]

    ENDINGS --> E1["这里停更好<br/>──────<br/>继续投入不值得了<br/>'够好了'就是成功<br/>确认不是假完成<br/>然后宣布胜利"]

    ENDINGS --> E2["这不是正确的旅程<br/>──────<br/>发现技术方案不可行<br/>杀掉项目+写复盘<br/>→ 不是失败而是成功<br/>因为避免了更大浪费<br/><br/>⚠️ 警惕沉没成本谬误<br/>投入越多越不想停<br/>但继续一个注定失败的项目<br/>只是推迟不可避免的"]

    ENDINGS --> E3["项目被取消了<br/>──────<br/>你无法控制的原因<br/>新领导、市场变化、<br/>资源削减<br/><br/>处理情绪：这很痛苦<br/>和管理者/信任的人谈<br/>理解更大的图景<br/><br/>然后和团队谈<br/>确保他们直接从你这里<br/>听到消息而非八卦<br/>给他们时间消化<br/>——他们可能对你<br/>作为'没保住项目的人'生气<br/><br/>干净地关闭项目<br/>能删的代码删掉<br/>写文档留给未来的人<br/><br/>为团队成员代言<br/>告诉他们的新管理者<br/>他们的成就<br/>写绩效评价时<br/>提到他们的贡献"]

    ENDINGS --> E4["这就是目的地！<br/>──────<br/>恭喜！<br/>确认达到了可衡量的目标<br/>用户能抓到宝可梦<br/>地基干净坚固<br/><br/>庆祝——聚会、礼物、<br/>全员会表彰<br/>给团队成员可见度<br/>内外部演讲和博客<br/><br/>做复盘——不只是找错<br/>也找做对了什么<br/>强化你想看到的文化"]

    style E1 fill:#fef7e0,stroke:#f9ab00
    style E2 fill:#fce8e6,stroke:#ea4335
    style E3 fill:#fce8e6,stroke:#ea4335
    style E4 fill:#e6f4ea,stroke:#34a853
```

---

## 本章总结

```mermaid
graph TB
    subgraph RECAP["第六章核心要点"]
        direction TB
        R1["作为项目领导<br/>你负责理解为什么停了<br/>并让它重新启动"]
        R2["作为组织领导<br/>你也能帮别人的项目重启"]
        R3["四种技术：<br/>理解解释、降低难度、<br/>获取组织支持、替代方案"]
        R4["给停滞的项目带来清晰：<br/>定义目标、明确角色、<br/>在需要时求助"]
        R5["不要宣布胜利<br/>除非项目真正完成<br/>代码完成只是一个里程碑"]
        R6["不管是否准备好<br/>有时候项目必须结束<br/>庆祝、复盘、收拾"]
    end

    style RECAP fill:#f8f9fa,stroke:#333
```

> **一句话总结：项目停滞有三类原因——外部阻塞、内部迷失、假完成。Staff 工程师的价值在于诊断真正的原因、用四种通用技术（理解解释、降低难度、组织支持、替代方案）解除阻塞，以及有勇气面对项目应该结束的现实——不管是宣布胜利、承认失败还是处理被取消，都要干净、有尊严、有复盘。**

---

**上一章**：[[ch5_领导大项目]] —— 如何启动和驾驶跨团队大项目
**下一章**：[[ch7_你现在是榜样了]] —— Part III 开始：以身作则和技术领导力的榜样效应
