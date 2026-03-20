# 《The Staff Engineer's Path》全书总览

> **作者**：Tanya Reilly | **出版**：O'Reilly, 2022年9月
> **副标题**：A Guide for Individual Contributors Navigating Growth and Change
> 一本关于如何在技术路线（IC Track）上持续成长为高级技术领导者的实操指南。

---

## 全书章节导航

| Part | 章节 | 文件 |
|---|---|---|
| **Part I: 大局观** | Ch1: 你在这里做什么？ | [[ch1_你在这里做什么]] |
| | Ch2: 三张地图 | [[ch2_三张地图]] |
| | Ch3: 创造大局观 | [[ch3_创造大局观]] |
| **Part II: 执行力** | Ch4: 有限的时间 | [[ch4_有限的时间]] |
| | Ch5: 领导大项目 | [[ch5_领导大项目]] |
| | Ch6: 为什么停滞了 | [[ch6_为什么停滞了]] |
| **Part III: 提升他人** | Ch7: 你现在是榜样了 | [[ch7_你现在是榜样了]] |
| | Ch8: 规模化正向影响 | [[ch8_规模化正向影响]] |
| | Ch9: 下一步是什么 | [[ch9_下一步是什么]] |

---

## 全书核心框架：三大支柱

```mermaid
graph TB
    Impact["🏛️ 影响力 Impact"]

    BP["大局观思维<br/>Big-picture Thinking<br/>──────────<br/>看到全局、避免局部最优<br/>制定技术愿景和战略<br/>预见未来3年的需求"]
    EX["项目执行力<br/>Project Execution<br/>──────────<br/>领导混乱的跨团队项目<br/>做正确的决策<br/>保持信息流动"]
    LU["提升他人<br/>Leveling Up<br/>──────────<br/>做榜样、树标准<br/>教学与指导<br/>推动文化变革"]

    TECH["技术知识与经验基础<br/>Tech Knowledge & Experience"]

    TECH --> BP
    TECH --> EX
    TECH --> LU
    BP --> Impact
    EX --> Impact
    LU --> Impact

    style Impact fill:#1a73e8,color:#fff,stroke:#333,stroke-width:2px
    style TECH fill:#a8d8ea,stroke:#333,stroke-width:2px
    style BP fill:#ffcc80,stroke:#333,stroke-width:1px
    style EX fill:#ffcc80,stroke:#333,stroke-width:1px
    style LU fill:#ffcc80,stroke:#333,stroke-width:1px
```

> 三大支柱建立在**扎实的技术知识和经验**基础之上。但光有技术还不够——你还需要"人类技能"（humaning skills）：沟通与领导力、驾驭复杂性、指导与赞助、框架化问题让他人关心、即使不觉得自己是领导也要像领导一样行动。
>
> 作者用了一个精彩的比喻：这些软技能就像**哥特式教堂的飞扶壁**——不替代墙体（你的技术判断力），但让建筑师可以建造更高、更宏伟的建筑。

---

## 全书章节流转关系

```mermaid
flowchart LR
    subgraph P1["Part I: 大局观"]
        direction TB
        C1["Ch1 定义角色<br/>你是谁？做什么？"]
        C2["Ch2 三张地图<br/>理解你的环境"]
        C3["Ch3 技术愿景<br/>创造方向"]
        C1 --> C2 --> C3
    end

    subgraph P2["Part II: 执行力"]
        direction TB
        C4["Ch4 时间管理<br/>选什么、不选什么"]
        C5["Ch5 领导项目<br/>跨团队协作"]
        C6["Ch6 排除障碍<br/>卡住时怎么办"]
        C4 --> C5 --> C6
    end

    subgraph P3["Part III: 提升他人"]
        direction TB
        C7["Ch7 做榜样<br/>以身作则"]
        C8["Ch8 规模化影响<br/>教学、审查、文化"]
        C9["Ch9 自我成长<br/>职业下一步"]
        C7 --> C8 --> C9
    end

    P1 --> P2 --> P3

    style P1 fill:#e8f0fe,stroke:#1a73e8
    style P2 fill:#fef7e0,stroke:#f9ab00
    style P3 fill:#e6f4ea,stroke:#34a853
```

---

## 重要提醒

> 这不是一本技术书。它是关于**技术路线上的领导力**。作者假设你已经具备（或正在学习）你领域的专业技术技能。本书教的是：如何在拥有技术能力的基础上，成为一个能产生超越个人代码贡献的巨大影响力的工程师。
