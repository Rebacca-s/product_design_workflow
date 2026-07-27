# Capability Registry — 能力注册中心

## 定位

能力注册中心是 `product_design_workflow` 项目的**能力映射中枢**。

它记录所有可用能力、对应的 Domain Agent、默认 Skill 和适用场景。Master Agent 在进行任务路由时，以本注册中心为权威依据。

## 设计原则

1. **集中登记**：所有能力、Agent、Skill 的映射关系在此集中维护
2. **支持扩展**：新增 Skill 或 Agent 时，优先在此登记，而不是直接修改所有路由规则
3. **单一真相来源**：Master Agent 路由时只参考此注册中心

---

## 能力映射表

| 能力 ID | 能力名称 | Domain Agent | 默认 Skill | 适用场景 | 涉及文件变更 |
|---------|----------|-------------|-----------|----------|-------------|
| `product-design` | 产品需求设计 | Product Agent | product-start | 新需求、需求修改、页面调整、规则设计、PRD 生成 | 是（outputs/） |
| `prd-review` | PRD 评审 | Review Agent | prd-review | 检查需求合理性、完整性、逻辑性、一致性、可研发性 | 否（只读评审） |
| `frontend-implement` | 前端实现 | Frontend Agent | frontend-implement | 页面开发、组件修改、前端代码实现 | 是（local_projects/） |
| `context-maintain` | 产品上下文维护 | Product Agent | context-maintain | 更新角色、页面、字段、业务规则 | 是（context/） |
| `architecture-review` | 架构分析 | Architecture Agent | architecture-review | 系统架构、模块划分、技术边界 | 否（分析输出） |
| `general-assist` | 通用问答 | General Agent | general-assist | 概念解释、简单讨论、不涉及项目变更 | 否 |

---

## Agent 注册表

| Agent ID | Agent 名称 | 定义文件 | 状态 |
|----------|-----------|----------|------|
| `master` | Master Agent | `.claude/agents/master-agent.md` | ✅ 已启用 |
| `product` | Product Agent | `.claude/agents/product-agent.md` | ✅ 已启用 |
| `review` | Review Agent | `.claude/agents/review-agent.md` | ✅ 已启用 |
| `frontend` | Frontend Agent | `.claude/agents/frontend-agent.md` | ✅ 已启用 |
| `general` | General Agent | `.claude/agents/general-agent.md` | ✅ 已启用 |
| `architecture` | Architecture Agent | `.claude/agents/architecture-agent.md` | 🚧 预留 |

---

## Skill 注册表

| Skill ID | Skill 名称 | 所属 Agent | 定义文件 | 状态 |
|----------|-----------|-----------|----------|------|
| `product-start` | 产品需求设计 | Product Agent | `.claude/skills/product-start/SKILL.md` | ✅ 已启用 |
| `prd-review` | PRD 评审 | Review Agent | `.claude/skills/prd-review/SKILL.md` | ✅ 已启用 |
| `frontend-implement` | 前端实现 | Frontend Agent | `.claude/skills/frontend-implement/SKILL.md` | ✅ 已启用 |
| `context-maintain` | 产品上下文维护 | Product Agent | — | 🚧 预留 |
| `architecture-review` | 架构分析 | Architecture Agent | — | 🚧 预留 |
| `general-assist` | 通用问答 | General Agent | — | 🚧 预留 |

---

## 自动触发关键词映射

以下关键词用于辅助 Master Agent 自动识别任务类型。**注意**：关键词匹配只是辅助手段，最终路由决策应结合上下文语义判断。

### → Product Agent (product-start)

**强信号**（命中即路由）：
- 需求、PRD、产品文档、功能设计、交互说明
- 新增、修改、调整、删除 + 页面/功能/按钮/字段/规则

**弱信号**（结合上下文判断）：
- 页面、字段、按钮、角色、权限、规则、状态、流程
- 订单、审核、结算、列表、筛选、弹窗、异常
- 业务逻辑、用户场景、操作流程
- "这里要改一下"、"这个不合理"、"帮我整理"

### → Review Agent (prd-review)

**强信号**：
- 检查、审核、评审、review
- 有没有问题、逻辑合理吗、有漏洞吗
- 是否完整、是否冲突

**弱信号**：
- 看看、帮我看、你觉得
- 能不能做、能做吗

### → Frontend Agent (frontend-implement)

**强信号**：
- 写页面、改前端、修改代码、实现
- 开发、落地、修复页面

**弱信号**：
- 根据 PRD、按照需求文档
- 组件、样式、路由

### → General Agent (general-assist)

**默认兜底**：不匹配以上任何能力时，进入通用问答。

---

## 扩展流程

当需要新增能力/Agent/Skill 时，按以下顺序操作：

1. **在本注册中心登记**：新增能力、Agent、Skill 的记录
2. **创建 Agent 定义文件**：在 `.claude/agents/` 下创建
3. **创建 Skill 定义文件**：在 `.claude/skills/` 下创建
4. **更新自动触发关键词**：在本文件的关键词映射表中补充
5. **更新 CLAUDE.md**：如有必要，补充自动触发规则
6. **不需要修改 Master Agent**：Master Agent 通过读取本注册中心获取最新映射

---

## 版本记录

| 日期 | 版本 | 变更内容 |
|------|------|----------|
| 2026-07-23 | v1.0 | 初始版本，定义 6 个能力、6 个 Agent、6 个 Skill |
