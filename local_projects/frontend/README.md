# frontend/

本地前端项目目录。

将真实前端项目克隆或复制到此目录下，例如：

```text
local_projects/frontend/duoai_pet_frontend/
local_projects/frontend/recording_app_frontend/
local_projects/frontend/yuanmeng_frontend/
```

## 使用方式

### 1. 放入项目

```bash
cd local_projects/frontend/
git clone <你的前端项目地址>
# 或直接复制项目文件夹到此处
```

### 2. 确保 .gitignore 生效

`local_projects/frontend/` 下的项目已在 `.gitignore` 中排除，不会提交到 product_design_workflow 仓库。

### 3. 在需求设计阶段使用

执行 `/product-start` 时，Claude Code 可以读取该目录下的前端项目，理解：

- 当前页面结构
- 路由配置
- 已有组件
- 页面文件位置
- 接口调用方式
- 样式规范
- 可复用模块

### 4. 在前端实现阶段使用

执行 `/frontend-implement` 时，Claude Code 可以直接修改该目录下的前端代码。
