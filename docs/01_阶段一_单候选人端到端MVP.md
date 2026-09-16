# 阶段 1：跑通单候选人端到端 MVP

## 目标

先实现最小可运行闭环：

**上传候选人材料 → 自动抽取结构化信息 → 生成候选人网页 → 每条关键信息可以追溯来源**

本阶段不追求复杂评分、雷达图、横向比较或高级 AI 评价，重点是把数据链路跑通。

---

## 3 人分工

### A：数据 / AI 解析

负责：

- 接收 PDF / CV / research statement / teaching statement / cover letter
- 抽取候选人结构化信息
- 输出统一 JSON
- 为每条关键信息保存来源
- 区分：
  - `verified`
  - `self_reported`
  - `uncertain`

建议最小字段：

```json
{
  "candidate_id": "",
  "basic_info": {},
  "education": [],
  "experience": [],
  "research_interests": [],
  "publications": [],
  "awards": [],
  "skills": [],
  "evidence": []
}
```

### B：后端 / 数据接口

负责：

- 读取结构化 JSON
- 提供 Candidate API
- 负责上传文件后的处理流程衔接
- 暂时可以不使用正式数据库
- 确保前端能稳定获取候选人数据

建议最小接口：

```text
POST /api/candidates/upload
GET  /api/candidates/:id
GET  /api/candidates
```

### C：前端 / 页面展示

负责：

- Candidate Profile 页面
- 展示基础信息、教育、经历、论文、奖项、标签
- 模块可点击展开
- 每条信息能查看来源
- 支持 Previous / Next Candidate 的基础框架

---

# 实施流程

## Step 1：先统一数据格式

三个人先一起确认 Candidate JSON Schema。

原则：

1. 页面需要什么字段，JSON 就提供什么字段。
2. 每条重要事实必须能够关联 evidence。
3. 不确定信息不要直接覆盖成确定事实。
4. 缺失字段允许为空，不要让 AI 编造。

建议 evidence：

```json
{
  "evidence_id": "ev_001",
  "source_type": "cv",
  "source_name": "CV.pdf",
  "page": 2,
  "url": null,
  "quote": "...",
  "status": "self_reported"
}
```

业务字段通过 `evidence_ids` 与证据关联。

---

## Step 2：A 完成 PDF → JSON

先只测试 1 名候选人。

输入：

```text
CV
Research Statement
Teaching Statement
Cover Letter
```

输出：

```text
candidate.json
```

要求：

- 信息结构统一
- 字段不存在就留空
- AI 推测内容必须标记为 `uncertain`
- 不要为了填满页面而生成不存在的信息

---

## Step 3：B 完成 JSON → API

后端第一版尽量简单。

流程：

```text
文件上传
   ↓
解析模块
   ↓
candidate.json
   ↓
后端保存
   ↓
Candidate API
```

本阶段不要求：

- 用户系统
- 权限
- 云数据库
- 多租户
- 高并发

---

## Step 4：C 完成 Candidate Profile

页面优先保证可读性。

建议结构：

```text
Candidate Name
Institution / Position
Research Areas
Tags

[Overview]

[Education]
[Experience]
[Research]
[Publications]
[Awards]

[Sources / Evidence]
```

交互：

- 点击模块展开详细信息
- 点击 evidence 查看原始来源
- URL 来源可以直接跳转网页
- PDF 来源至少显示文件名 + 页码

---

## Step 5：三人联调

必须真正从上传开始运行一次：

```text
Upload PDF
→ Parse
→ Save JSON
→ API
→ Candidate Profile
→ Click Evidence
```

不能只展示提前写好的静态页面。

---

# 可直接交给 Codex / AI 的提示词

```text
我们正在实现一个 Candidate Information Extraction & AI-Assisted Analysis 的课程项目。

当前只实现阶段 1 的最小可行版本，不要提前扩展阶段 2、3 的功能。

目标是跑通：

上传候选人材料
→ 自动抽取结构化信息
→ 保存为统一 Candidate JSON
→ 后端读取
→ 前端生成 Candidate Profile
→ 每条重要信息可以追溯到来源

请先阅读当前项目代码，并基于已有技术栈实现，不要为了这个功能重构整个项目。

核心要求：

1. 定义一个清晰、稳定的 Candidate JSON Schema。
2. 至少支持：
   - basic information
   - education
   - work/research experience
   - research interests
   - publications
   - awards
   - skills/tags
3. 每条关键记录需要关联 evidence。
4. evidence 至少保存：
   - source type
   - source name
   - page 或 URL
   - evidence status
5. 对无法确认的信息：
   - 不要编造
   - 标记为 uncertain 或 missing
6. 第一版可以直接保存 JSON，不必引入复杂数据库。
7. 前端实现一个清晰的 Candidate Profile 页面。
8. 页面中的来源可以点击或查看。
9. 必须支持从上传材料开始的真实端到端流程，而不是只使用静态 mock data。

暂时不要实现：
- 雷达图
- 数值评分系统
- Candidate 横向比较
- 自然语言搜索
- Interview Question Skill
- 复杂 AI recommendation
- 批量大规模处理

请先检查现有项目结构，然后给出最小修改方案，再开始实现。
```

---

# 阶段 1 验收标准

完成以下流程即可进入阶段 2：

- [ ] 可以上传至少一个候选人的 PDF 材料
- [ ] 能自动生成 Candidate JSON
- [ ] JSON 字段结构稳定
- [ ] 页面能正常展示候选人
- [ ] 教育 / 经历 / 论文 / 奖项等主要模块正常
- [ ] 至少一条信息可以追溯到 PDF 页码或网页
- [ ] 不确定信息有明确状态
- [ ] 缺失信息不会被 AI 自动补造
- [ ] 整个流程可以从上传开始现场演示
