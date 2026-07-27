# Agent 与 Skill 调用规则

## 核心原则

> Domain Agent 承接任务，Skill 执行具体工作流。Agent 决定"做什么"，Skill 决定"怎么做"。

---

## 一、调用层级

```text
Master Agent（识别/路由/编排）
    ↓
Domain Agent（领域决策）
    ↓
Skill（工作流执行）
    ↓
规则、模板、上下文（数据支撑）
```

---

## 二、Agent 调用 Skill 的规范

### 2.1 调用流程

1. Domain Agent 确认任务属于自身领域
2. Domain Agent 选择对应的 Skill
3. Skill 开始执行前，确认调用来源
4. Skill 按定义的工作流执行
5. Skill 完成后，结果返回 Domain Agent
6. Domain Agent 评估是否需要串联其他 Agent

### 2.2 调用来源检查

每个 Skill 开始执行时，必须输出：

```markdown
## Skill 调用确认

| 项目 | 内容 |
|------|------|
| 调用来源 | Master Agent → [Domain Agent] |
| 任务类型 | [任务类型] |
| 执行 Skill | [Skill 名称] |
| 当前阶段 | [阶段名称] |
```

### 2.3 职责边界

| Agent | 可以调用的 Skill | 不能做的事 |
|-------|-----------------|-----------|
| Product Agent | product-start, context-maintain | 不能改前端代码，不能评审（那是 Review Agent 的事） |
| Review Agent | prd-review | 不能直接改写需求（除非被授权），不能改代码 |
| Frontend Agent | frontend-implement | 不能重新定义需求，不能自行做产品决策 |
| General Agent | general-assist | 不能修改项目文件 |

---

## 三、路由状态展示格式

### 3.1 标准格式（复杂任务）

```markdown
## 本次任务路由

| 项目 | 内容 |
|------|------|
| 任务类型 | [产品需求设计/产品需求修改/PRD评审/前端实现/通用问答] |
| 承接 Agent | [Product Agent / Review Agent / Frontend Agent / General Agent] |
| 执行 Skill | [product-start / prd-review / frontend-implement / 无] |
| 当前阶段 | [阶段名称] |
| 是否修改文件 | [是/否] |
| 后续阶段 | [如有，列出后续阶段] |
```

### 3.2 压缩格式（简单问答）

```text
任务类型：通用解释 | Agent：General Agent | Skill：无
```

### 3.3 多阶段任务格式

```markdown
## 本次任务路由

| 项目 | 内容 |
|------|------|
| 任务类型 | 多阶段任务 |
| 阶段数 | [N] 个阶段 |
| 承接 Agent | Review Agent → Product Agent → Frontend Agent |
| 执行 Skill | prd-review → product-start → frontend-implement |
| 当前阶段 | 第 1 阶段：PRD 评审 |
| 是否修改文件 | 是（后续阶段） |
```

---

## 四、阶段状态展示

所有较复杂任务都需要输出阶段状态。格式按 `rules/阶段状态提示规则.md` 执行。

阶段状态必须让用户知道：
- 当前在哪里
- 已完成什么
- 接下来做什么
- 是否需要确认

---

## 五、Agent 间串联规则

### 5.1 标准串联

```text
Product Agent（设计）
    ↓
Review Agent（评审）
    ↓
Product Agent（修订）
    ↓
Frontend Agent（实现）
```

### 5.2 串联触发条件

| 触发条件 | 串联动作 |
|----------|----------|
| Product Agent 完成 PRD 生成 | 建议进入 Review Agent |
| Review Agent 发现问题 | 返回 Product Agent 修订 |
| 需求确认无误 | 可进入 Frontend Agent |
| Frontend Agent 发现需求问题 | 返回 Review Agent 评估 → Product Agent 修订 |

### 5.3 串联规则

1. 串联由 Master Agent 编排，不由单个 Agent 自行决定
2. 每个阶段完成后，Domain Agent 向 Master Agent 报告结果
3. Master Agent 根据结果决定下一阶段
4. 用户可随时中断串联，要求停留在当前阶段

---

## 六、Skill 执行约束

### 6.1 Skill 必须做的事

1. 确认调用来源（Master Agent / Domain Agent）
2. 确认任务类型是否属于自身职责
3. 如果不属于，返回 Master Agent 重新路由
4. 按定义的工作流步骤执行
5. 输出阶段状态卡
6. 完成后报告结果

### 6.2 Skill 不能做的事

1. 不能因为缺少用户输入的 `/skill-name` 而拒绝执行
2. 不能跳过工作流中的必要步骤
3. 不能越过 Domain Agent 直接面向用户（路由状态由 Master Agent 统一展示）
4. 不能调用不属于自身领域的能力

---

## 七、新增 Skill 的接入规范

当需要新增 Skill 时，必须：

1. 在 `capability-registry.md` 登记
2. 创建 `SKILL.md`，包含：
   - 调用来源检查
   - 依赖规则文件引用
   - 阶段状态提示
   - 工具调用透明
3. 如果属于已有 Agent，更新该 Agent 定义文件
4. 如果属于新 Agent，创建 Agent 定义文件并在 registry 登记
5. 在 `rules/任务路由规则.md` 中补充自动触发关键词

---

## 版本记录

| 日期 | 版本 | 变更内容 |
|------|------|----------|
| 2026-07-23 | v1.0 | 初始版本 |
