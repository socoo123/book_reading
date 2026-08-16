# AGENTS.md

本仓库是技术书籍读书笔记集合。

## 《算法导论》笔记（IntroductionToAlgorithms/）

当用户说「**生成 / 优化 / 审阅第 N 章**」时：

1. 先完整阅读 **`IntroductionToAlgorithms/Plan.md`**——它是所有章节的统一准则与唯一权威流程（内容准则、Mermaid 图准则、代码准则、执行顺序、已完成章节范例）。
2. 按其执行：Step 0 用 `pdftotext` 提取第四版原书对应章节（PDF 在 `/Users/zy/Downloads/IntroductiontoAlgorithms4th.pdf`，PDF 页 = 书页 + 22）→ 修错/撰写 → Mermaid 重绘 → Java+Python 双实现 → 按统一骨架重组。
3. 产出文件命名：`ChapterNN_章名_深度版.md`；替换旧版后删除旧文件。
4. **最终关卡（必做）**：笔记中的代码块原样抽出编译运行 + 随机对拍验证；全部 Mermaid 块用 `mermaid.parse` 验证可解析。
