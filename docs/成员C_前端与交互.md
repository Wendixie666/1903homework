# 成员 C：前端、交互与可视化

## 核心目标

负责把结构化 Candidate 信息做成一个清晰、可操作的网页。

第一阶段最重要的目标是：

> 用户打开网页后，可以快速理解一个候选人，并点击查看每条信息的来源。

---

## 阶段 1：Candidate Profile 页面

### 1. 页面顶部

展示：

```text
姓名
当前机构 / 职位
研究方向
核心标签
个人主页
GitHub
```

例如：

```text
张三
Assistant Professor · XXX University

LLM · Software Engineering · Testing
```

---

### 2. 核心信息模块

至少展示：

- Basic Information
- Education
- Work Experience
- Research Interests
- Publications
- Awards
- Funding
- Skills / Tags

每个模块支持：

```text
点击展开 / 收起
```

避免整页一次展示太多文字。

---

### 3. Evidence 可追溯

重要记录旁边增加来源入口，例如：

```text
PhD · HKUST
[CV p.1] [Personal Website]
```

点击后可以：

- 打开原 PDF 对应来源
- 打开个人主页
- 打开论文页面
- 打开 GitHub
- 查看原始 evidence

这是 MVP 中非常重要的一部分。

---

### 4. Candidate 浏览

加入：

```text
← Previous
Candidate List
Next →
```

让 reviewer 可以快速浏览不同候选人。

---

## 阶段 2：搜索与筛选界面

和 B 的搜索功能连接。

支持：

```text
Search candidates...
```

以及标签：

```text
[LLM]
[Robotics]
[Software Testing]
[Computer Vision]
```

用户点击标签即可筛选。

同时加入自然语言搜索框，例如：

```text
找有 LLM 和 testing 经历的人
```

---

## 阶段 2：Candidate Compare

支持选择：

```text
Candidate A
Candidate B
```

然后横向展示：

```text
Education
Research
Publications
Awards
Funding
Skills
Teaching
```

重点是帮助 reviewer 快速看出差异。

---

## 阶段 2：冲突与亮点展示

对于 A 检测到的内容：

### Strength

例如：

```text
★ Strong publication record in software testing
```

### Conflict

例如：

```text
⚠ CV 与 Research Statement 中项目年份不一致
```

点击可以展开详细证据。

---

## 阶段 3：视觉增强

基础页面稳定以后再做：

- 雷达图
- 候选人在某维度的相对位置
- 更漂亮的 timeline
- 卡片展开动画
- Candidate comparison 可视化
- AI summary
- 面试追问问题
- 响应式布局

雷达图和复杂评分不要放在第一阶段。

---

## 页面建议结构

```text
┌──────────────────────────────┐
│ Search / Filter              │
├──────────────────────────────┤
│ Candidate Name               │
│ Institution · Position       │
│ Tags                         │
├──────────────────────────────┤
│ Overview                     │
├──────────────────────────────┤
│ Education                    │
├──────────────────────────────┤
│ Experience                   │
├──────────────────────────────┤
│ Publications                 │
├──────────────────────────────┤
│ Awards & Funding             │
├──────────────────────────────┤
│ Evidence / Sources           │
└──────────────────────────────┘
```

---

## 和其他成员的接口

### 从成员 A 获取

Candidate JSON 字段。

### 从成员 B 获取

- Candidate list
- Candidate detail
- Search results
- Compare data

前端只负责展示和交互，不重复做数据抽取。

---

## 第一阶段完成标准

满足以下条件即可：

- [ ] 可以展示一个完整 Candidate Profile
- [ ] 页面结构清晰
- [ ] 各模块可以展开 / 收起
- [ ] 可以点击来源
- [ ] 可以切换 Candidate
- [ ] 可以展示 tags
- [ ] 能和 B 提供的数据接口连接

第一阶段重点是“清楚、可用、能演示”，不要先花大量时间做动画和复杂视觉效果。
