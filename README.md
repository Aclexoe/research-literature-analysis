# research-literature-analysis

> Deep reading of scientific papers, cross-paper comparison, research lineage mapping,
> consensus vs. controversy analysis, research gap mining and personalized research advice.

一个面向深度文献阅读的 Skill / Prompt 系统：不是摘要器，而是科研导师。
帮你回答三个问题：**Q1 研究什么？Q2 研究到哪里了？Q3 对我有什么用？**

**一句话定位：这个仓库 = 一份通用 Prompt（`SKILL.md`），在哪里都能用。**
Cursor 的 skills 目录只是其中一种用法；豆包 / ChatGPT / Claude 网页版 /
Grok / Kimi / 文心 / 通义里，把 `SKILL.md` 全文粘进 System Prompt 或第一条消息即可，
效果完全一样，不需要任何安装。

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
├─ SKILL.md                  # 主文件：唯一的通用 Prompt，全文复制即用
├─ README.md                 # 本文件
├─ LICENSE                   # MIT
└─ references/               # 大模板按需使用（Agent 可选读，避免主 Prompt 过长）
   ├─ paper-card.md          # 单篇精读空表
   ├─ report-template.md     # 最终研究报告骨架（11 章）
   └─ related-work.md        # Related Work / Review 句式库
```

## 用法总览（选一种即可，效果一样）

| 场景 | 做法 | 说明 |
|---|---|---|
| 豆包 / ChatGPT / Claude / Kimi 等网页版 | 打开 `SKILL.md` 全选复制，粘到第一条消息，换行加一句"明白了就回复：已加载" | 一次性，无需注册、无需安装 |
| 对话已开始、想中途启用 | 同上，把全文粘进输入框发送即可 | 上下文长度够就能用 |
| Cursor / Claude Code（支持 skills） | 把本仓库放进 skills 目录（见下） | 自动触发，也可手动点名调用 |
| 长期复用（豆包/Coze/扣子/GPTs/Dify） | 把 `SKILL.md` 正文粘进智能体的"人设 / System Prompt / 技能" | 之后每次对话自动生效 |
| 本地脚本 / API 调用 | 把 `SKILL.md` 内容作为 `system` 消息传入 | 任何模型通用 |

**提示词只有约 6k token，主流模型（GPT-4 / Claude / 豆包 / Kimi K2 / Qwen）
上下文都轻松容纳，整份粘贴即可，不用删减。**

---

## 方法一：通用 AI（豆包 / ChatGPT / Claude / Kimi / Grok / 通义 / 文心）——复制粘贴

1. 打开本仓库的 [`SKILL.md`](./SKILL.md)，点击右上角 `Raw`，全选复制全文。
2. 新建一个对话，第一条消息粘贴全文，末尾加一句：

```text
明白了就回复：已加载。
我的研究问题是：xxx，阶段是[选题/开题/方法设计/写Related Work/投稿前查新/组会汇报]，
数据类型是：xxx。
```

3. 看到"已加载"后，上传 PDF / 粘 DOI / 贴 arXiv 链接，正常说话：

```text
精读附件这篇PDF，输出 Paper Card + 一句话总结 + 对我的帮助
```

```text
对比附件这5篇，按技术路线分类，输出文献矩阵 + 共识C1-C3 + 分歧D1-D2 + Real Gap
```

```text
基于上面这些文献，帮我设计 baseline梯度、必须用的metrics及原因、
主实验+消融+泛化实验清单，再给3个候选创新方向
```

```text
只输出快速版：背景+共识+分歧+Gap+对我的帮助+总结
```

附件传不进 prompt 的模板（如需用空表格式）：
把 [`references/paper-card.md`](./references/paper-card.md) /
[`references/report-template.md`](./references/report-template.md)
内容同样复制粘贴给它，说"按这个格式填"即可。

## 方法二：长期复用（豆包智能体 / Coze / 扣子 / GPTs / Dify / FastGPT）

本质都是"把 SKILL.md 存成系统人设"，配一次，永久生效：

1. 建智能体 / GPTs / 知识库应用。
2. 把 `SKILL.md` 全文（含头部 `name` + `description`，含 0-9 章正文）
   粘进"人设与回复逻辑 / System Prompt / Instructions"。
3. 可选：把 `references/` 下三个 `.md` 传为知识库文件或开场白附件，
   并加一句"需要空表格式时读取知识库对应文件"。
4. 发布。之后每次对话直接说需求，无需重复粘贴。

## 方法三：Cursor（skills 自动触发）

1. 复制本仓库到 Cursor skills 目录：

```powershell
# 假设仓库 clone 到 F:\research-literature-analysis
mkdir "$HOME\.cursor\skills\research-literature-analysis\references" -Force
Copy-Item "F:\research-literature-analysis\SKILL.md" "$HOME\.cursor\skills\research-literature-analysis\SKILL.md" -Force
Copy-Item "F:\research-literature-analysis\references\*" "$HOME\.cursor\skills\research-literature-analysis\references\" -Force
```

或者直接 clone 到 skills 目录：

```powershell
git clone https://github.com/Aclexoe/research-literature-analysis.git "$HOME\.cursor\skills\research-literature-analysis"
```

2. 重启 Cursor，让它重新索引 skills。
3. 验证（在 Agent 对话框输入）：

```text
你现在有哪些可用的 skill？research-literature-analysis 在吗？
```

返回了 description 即加载成功。

> `SKILL.md` 头部的 `name` + `description` 是触发器，不要改成纯中文简介，
> 不要删 DOI / arXiv / gap / baseline 等英文关键词，否则自动触发会失效。

## 各平台实测说明

| 平台 | 附件/文件 | 超长 PDF | 备注 |
|---|---|---|---|
| Cursor | 直接读工作区 + 附件 | 无压力 | skills 自动触发最佳 |
| 豆包 | 支持 PDF/Word/图片上传 | 先传全文，读漏时追问页码 | 对中文指令响应好，建议中文提问 |
| ChatGPT（GPT-4/5） | Plus 可传 PDF | 大文件分段传，或先传文字版 | 联网查 DOI/arXiv 最强，可开联网补元数据 |
| Claude 网页版 | PDF/图片/文本均可 | 200k 上下文，长综述首选 | 中文 + 证据引用最稳 |
| Kimi | 长文档是强项 | 百页 thesis 可直接扔 | 配合联网查新好用 |
| API（OpenAI/Claude/豆包） | 自行拼接 | 自行分段 | `SKILL.md` 作 `system` 消息传入即可 |

通用建议：只给了标题/摘要时，主动要求模型降级输出
（`仅基于摘要，结论为低可信度`），这是 SKILL.md 第 0 节内置的规则。

## 详细操作示例（各平台通用）

### 单篇精读

```text
精读附件这篇PDF。
我的研究问题是：用Transformer做光伏功率预测，阶段是开题。
输出 Paper Card + 一句话总结 + 对我的帮助。
```

### 横向比较（2-10 篇）

```text
分析附件这6篇。
都是做医学影像分类的，帮我按技术路线分类，
输出文献矩阵 + 共识C1-C3 + 分歧D1-D2 + Real Gap。
```

### 大批量综述（10-30+ 篇）

```text
做综述，文献共27篇（分批上传）。
先做索引和分类，再重点读奠基/转折/SOTA 5篇，
其余一句话定位，最后输出完整研究报告第1-11章。
```

### 为自己的研究服务

```text
基于上面这些文献，帮我设计：
1. baseline梯度 2. 必须用的metrics及原因 3. 主实验+消融+泛化实验清单
4. 3个候选创新方向，每个含问题/想法/方法/数据/风险
我的数据是：3年风电场SCADA时序数据，无外部验证集。
```

详细格式说明见 `references/` 下三个模板。

## 防幻觉规则（所有平台生效）

1. 重要判断必须带出处，无信息写 `论文未明确报告`，禁止编 DOI / 数据 / 指标。
2. 每个结论打标签：`【论文事实】` / `【作者观点】` / `【综合判断】` / `【研究假设】`。
3. 跨不同数据集不直接比数值大小，不忽略年份和 baseline 公平性排优劣。
4. 只有摘要时降级：数值结论一律视为低可信度。

## License

MIT
