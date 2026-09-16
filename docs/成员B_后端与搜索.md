# 成员 B：后端、搜索与系统逻辑

## 核心目标

负责把 A 生成的结构化 Candidate 数据变成可查询、可筛选、可比较的系统能力。

第一阶段最重要的目标是：

> Candidate JSON 可以被网页读取，并且用户能够搜索、筛选和切换候选人。

---

## 阶段 1：先完成最小可行版本

### 1. 读取 Candidate JSON

建立简单的数据访问层。

第一版不一定需要数据库，可以直接：

```text
data/
├── candidate_001.json
├── candidate_002.json
└── candidate_003.json
```

提供基础接口，例如：

```text
GET /candidates
GET /candidate/:id
```

### 2. 实现基础筛选

至少支持：

- research area
- skills
- institution
- education
- experience
- publication keywords
- tags

例如：

```text
LLM
Software Engineering
Robotics
Computer Vision
Formal Verification
```

用户选择多个标签后，可以快速筛选候选人。

### 3. 支持 Candidate 切换

至少支持：

```text
Previous Candidate
Next Candidate
Candidate List
```

让前端不用重新上传文件就能浏览不同候选人。

### 4. 为对比功能准备接口

例如：

```text
GET /compare?id=A&id=B
```

或者直接返回两个 Candidate 的结构化数据。

---

## 阶段 2：自然语言搜索

实现一个自然语言搜索框。

例如用户输入：

```text
找有 LLM 和 software testing 经历的人
```

转换为：

```json
{
  "research_area": ["LLM"],
  "skills": ["software testing"]
}
```

再调用普通筛选逻辑。

重点是：

> 自然语言搜索只是把用户需求转成结构化筛选条件，不要重新写一整套搜索系统。

---

## 阶段 2：横向比较

支持选择两个或多个 Candidate。

比较维度可以包括：

- Education
- Research areas
- Publications
- Awards
- Funding
- Skills
- Teaching
- GitHub / Projects

第一版只需要展示差异，不需要自动判断“谁更好”。

---

## 阶段 3：高级功能

基础功能稳定之后，再考虑：

- 搜索结果排序
- 多条件组合查询
- AI 自动生成 comparison summary
- 保存筛选条件
- Candidate shortlist
- 人工修改结构化数据
- 修改后重新保存
- 批量 Candidate 管理

---

## 和其他成员的接口

### 从成员 A 获取

统一结构：

```text
candidate.json
```

不要在后端重新解析 CV。

### 给成员 C 提供

前端需要的接口：

```text
candidate list
candidate detail
search results
comparison data
```

---

## 第一阶段完成标准

满足以下条件即可：

- [ ] 可以读取多个 Candidate JSON
- [ ] 可以返回 Candidate 列表
- [ ] 可以查看单个 Candidate
- [ ] 可以按标签筛选
- [ ] 可以切换不同 Candidate
- [ ] 前端可以直接调用
- [ ] 可以选择两名 Candidate 为后续比较做准备

第一阶段不要优先做复杂推荐系统、向量数据库或复杂排序算法。
