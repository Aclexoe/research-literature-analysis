# research-literature-analysis

> Deep reading of scientific papers, cross-paper comparison, research lineage mapping,
> consensus vs. controversy analysis, research gap mining and personalized research advice.

一个面向深度文献阅读的 Skill / Prompt 系统：不是摘要器，而是科研导师。
帮你回答三个问题：**Q1 研究什么？Q2 研究到哪里了？Q3 对我有什么用？**

适用于 Cursor / Claude / 任何支持 Skills 或 System Prompt 的 Agent。

---

## 功能

- 单篇论文深度精读（Paper Card：问题 / 动机 / Pipeline / 数据 / Baseline / 消融 / 局限）
- 多篇横向综合（按技术路线聚类 + 因果演进链 + 文献矩阵，拒绝按 1、2、3 罗列）
- 共识（Consensus）与分歧（Disagreement）识别，证据分级
- Research Gap 挖掘（区分 Real Gap / Fake Gap，每个 Gap 必须有证据论文）
- 个性化科研建议（baseline 梯度 / metrics / 实验清单 / 创新方向 / 避坑）
- Related Work / Literature Review / 开题 / 查新 / 组会汇报支持

## 目录结构

```text
research-literature-analysis/
├─ SKILL.md                  # 主文件（name + description 触发器 + 全流程 0-9 章）
├─ README.md                 # 本文件
└─ references/               # 大模板按需读取，避免 SKILL.md 过长
   ├─ paper-card.md          # 单篇精读空表
   ├─ report-template.md     # 最终研究报告骨架（11 章）
   └─ related-work.md        # Related Work / Review 句式库
```

## 安装（Cursor）

1. 复制本仓库到 Cursor skills 目录：

```powershell
# 假设仓库 clone 到 F:\research-literature-analysis
mkdir "$HOME\.cursor\skills\research-literature-analysis\references" -Force
Copy-Item "F:\research-literature-analysis\SKILL.md" "$HOME\.cursor\skills\research-literature-analysis\SKILL.md" -Force
Copy-Item "F:\research-literature-analysis\references\*" "$HOME\.cursor\skills\research-literature-analysis\references\" -Force
```

或者直接 clone 到 skills 目录：

```powershell
git clone https://github.com/<你的用户名>/research-literature-analysis.git "$HOME\.cursor\skills\research-literature-analysis"
```

2. 重启 Cursor，让它重新索引 skills。
3. 验证（在 Agent 对话框输入）：

```text
你现在有哪些可用的 skill？research-literature-analysis 在吗？
```

返回了 description 即加载成功。

> `SKILL.md` 头部的 `name` + `description` 是触发器，不要改成纯中文简介，
> 不要删 DOI / arXiv / gap / baseline 等英文关键词，否则自动触发会失效。

## 用法

不需要点按钮，正常说话即可。话里包含 `论文 / PDF / DOI / arXiv / 找Gap / 共识分歧 / baseline / Related Work` 会自动命中。

手动触发：

```text
用 research-literature-analysis 精读这篇论文，并找 Gap
```

```text
用 research-literature-analysis 对比附件这5篇，输出共识/分歧/矩阵
```

每次开头加一句背景（否则"对我的帮助"只能说空话）：

```text
我的研究问题是：xxx，阶段是[选题/开题/方法设计/写Related Work/投稿前查新/组会汇报]，
数据类型是：xxx。
```

### 四种常用操作

**单篇精读：**

```text
用 research-literature-analysis 精读附件这篇PDF。
我的研究问题是：用Transformer做光伏功率预测，阶段是开题。
输出 Paper Card + 一句话总结 + 对我的帮助。
```

**横向比较（2-10 篇）：**

```text
用 research-literature-analysis 分析附件这6篇。
都是做医学影像分类的，帮我按技术路线分类，
输出文献矩阵 + 共识C1-C3 + 分歧D1-D2 + Real Gap。
```

**大批量综述（10-30+ 篇）：**

```text
用 research-literature-analysis 做综述，文献在 F:\papers\ 共27篇。
先做索引和分类，再重点读奠基/转折/SOTA 5篇，
其余一句话定位，最后输出完整研究报告第1-11章。
```

**为自己的研究服务：**

```text
基于上面这些文献，用 research-literature-analysis 帮我设计：
1. baseline梯度 2. 必须用的metrics及原因 3. 主实验+消融+泛化实验清单
4. 3个候选创新方向，每个含问题/想法/方法/数据/风险
我的数据是：3年风电场SCADA时序数据，无外部验证集。
```

输出太长时：

```text
只输出快速版：背景+共识+分歧+Gap+对我的帮助+总结
```

详细操作说明见 `references/` 下三个模板。

## 防幻觉规则

1. 重要判断必须带出处，无信息写 `论文未明确报告`，禁止编 DOI / 数据 / 指标。
2. 每个结论打标签：`【论文事实】` / `【作者观点】` / `【综合判断】` / `【研究假设】`。
3. 跨不同数据集不直接比数值大小，不忽略年份和 baseline 公平性排优劣。
4. 只有摘要时降级：数值结论一律视为低可信度。

## License

MIT
