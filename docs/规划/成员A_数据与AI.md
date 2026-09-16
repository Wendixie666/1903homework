# 成员 A：数据与 AI

## 核心目标

负责把候选人的原始材料转换成统一、可供系统使用的结构化数据。

第一阶段最重要的目标是：

> 输入 CV / teaching statement / research statement / cover letter，输出统一的 Candidate JSON。

---

## 阶段 1：先完成最小可行版本

### 1. 设计统一数据结构

建议至少包含：

```text
candidate
├── basic_info
├── education
├── experience
├── research_interests
├── publications
├── awards
├── funding
├── skills
├── tags
└── evidence
```

每条重要信息尽量保存：

- 内容
- 来源文件
- 页码或原始位置
- 原文片段
- evidence status

例如：

```json
{
  "institution": "HKUST",
  "degree": "PhD",
  "source": "CV",
  "page": 1,
  "evidence_status": "provided_material"
}
```

### 2. 完成 PDF / 文档信息提取

至少提取：

- 姓名
- 当前单位与职位
- 研究方向
- 教育经历
- 工作经历
- 论文
- 奖项
- Funding / Grants
- GitHub / 个人主页 / 项目链接
- 技能标签

### 3. 输出统一 JSON

先不追求复杂数据库。

只需要保证：

```text
Candidate Files
      ↓
AI / Parser
      ↓
candidate.json
```

这个 JSON 可以直接交给 B 和 C 使用。

---

## 阶段 2：加入 AI 分析

在基础事实提取稳定之后，再增加：

- strengths
- research summary
- teaching summary
- 材料之间的矛盾检测
- unsupported claims
- uncertain information
- verified / self-reported / uncertain 状态

AI 生成内容必须和原始事实区分开。

例如：

```json
{
  "type": "ai_analysis",
  "content": "The candidate has a coherent research trajectory...",
  "based_on": ["education_01", "publication_03"]
}
```

---

## 阶段 3：后续优化

如果前两阶段已经稳定，再做：

- PDF 前置结构化，减少重复 token 消耗
- 缓存已经解析过的文件
- 同义标签归一化
  - `LLM`
  - `Large Language Model`
  - `large language models`
- 更准确的 citation / evidence 定位
- 为 AI 面试问题生成提供结构化输入

---

## 和其他成员的接口

### 给成员 B

提供稳定的：

```text
candidate.json
```

B 不需要重新解析 PDF。

### 给成员 C

保证 JSON 字段稳定，让前端可以直接渲染：

```text
candidate.basic_info
candidate.education
candidate.publications
candidate.tags
candidate.evidence
```

---

## 第一阶段完成标准

满足以下条件即可：

- [ ] 可以输入至少 1 个候选人的材料
- [ ] 自动生成 Candidate JSON
- [ ] 基本信息、教育、经历、论文、奖项可以正确展示
- [ ] 每条重要信息保留来源
- [ ] JSON 可以直接被前端读取
- [ ] 不需要人工复制粘贴整理

第一阶段不要优先做复杂评分、雷达图或很长的 AI 评价。
