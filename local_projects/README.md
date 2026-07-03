# local_projects/

本地项目接入区。用于放置真实的前端项目源码，让 Claude Code 在需求设计和前端实现阶段读取和修改。

## 目录结构

```text
local_projects/
└── frontend/           # 本地前端项目
    ├── .gitkeep
    ├── README.md
    ├── duoai_pet_frontend/       # 示例：哆爱宠前端项目
    ├── recording_app_frontend/   # 示例：录音 App 前端项目
    └── yuanmeng_frontend/        # 示例：元梦客前端项目
```

## 作用

1. 需求澄清阶段：Claude Code 理解当前已有页面、组件、路由、接口
2. 前端实现阶段：Claude Code 直接修改页面代码
3. 作为本地源码工作区使用

## 与 context/products/ 的区别

| 目录 | 是否提交 | 作用 |
|------|----------|------|
| `context/products/` | 提交 | 沉淀业务背景、页面地图、角色权限、流程、字段、接口说明 |
| `local_projects/frontend/` | 不提交 | 放真实前端源码，让 Claude Code 读取和修改代码 |

## 重要

- `local_projects/frontend/` 下的真实前端项目**不提交**到 product_design_workflow 仓库
- 该目录已在 `.gitignore` 中排除，仅保留 `.gitkeep` 和本 README.md
