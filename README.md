# Vue 3 OpenCode Template

基于 Vue 3 + Vite 的现代化前端项目模板，集成 OpenCode 多 Agent 开发系统。

## 项目技术栈

| 技术 | 说明 |
|------|------|
| Vue 3 | 渐进式前端框架 (Composition API + `<script setup>`) |
| Vite | 下一代前端构建工具 |
| Pinia | 轻量级状态管理方案 |
| Vue Router | 官方路由管理器 |
| Element Plus | 基于 Vue 3 的组件库 |
| Less | CSS 预处理器 |

## 项目结构

```
vue3-opencode-template/
├── .opencode/                 # OpenCode 多 Agent 配置
│   ├── agents/                # Agent 角色定义
│   │   ├── coreCoordinatorAgent.md       # 核心协调 Agent
│   │   ├── requirementAnalysisAgent.md   # 需求分析 Agent
│   │   ├── experienceVisualDesignAgent.md # 体验视觉设计 Agent
│   │   ├── techArchitectureAgent.md     # 技术架构 Agent
│   │   ├── coreDevelopmentAgent.md       # 核心开发 Agent
│   │   ├── businessDevelopmentAgent.md   # 业务开发 Agent
│   │   ├── testQualityAssuranceAgent.md  # 测试质量保障 Agent
│   │   └── engineeringDevOpsAgent.md     # 工程化 DevOps Agent
│   └── package.json          # OpenCode 依赖配置
├── src/                       # 源代码目录
│   ├── components/           # Vue 组件
│   ├── views/                # 页面视图
│   ├── stores/               # Pinia 状态管理
│   ├── router/               # 路由配置
│   ├── utils/                # 工具函数
│   ├── api/                  # API 接口
│   ├── assets/               # 静态资源
│   ├── App.vue               # 根组件
│   └── main.js               # 入口文件
├── docs/                     # 需求文档目录
├── package.json              # 项目依赖配置
└── vite.config.js            # Vite 配置
```

## 快速开始

### 安装依赖

```bash
npm install
```

### 开发模式

```bash
npm run dev
```

### 构建生产版本

```bash
npm run build
```

### 预览生产版本

```bash
npm run preview
```

## OpenCode 多 Agent 开发系统

本项目已深度集成 OpenCode 多 Agent 开发框架，配置了 8 个专业 Agent 角色，支撑完整的软件开发流程。

### Agent 角色矩阵

| Agent | 角色 | 职责 |
|-------|------|------|
| `coreCoordinatorAgent` | 核心协调 | 任务拆解、调度、冲突解决、质量验收 |
| `requirementAnalysisAgent` | 需求分析 | 需求深度解析、PRD 文档输出 |
| `experienceVisualDesignAgent` | 体验设计 | 用户流程、交互逻辑、UI/UX 设计 |
| `techArchitectureAgent` | 技术架构 | 技术选型、架构设计、代码规范 |
| `coreDevelopmentAgent` | 核心开发 | 核心模块、复杂组件、代码评审 |
| `businessDevelopmentAgent` | 业务开发 | 业务页面、通用功能、开发自测 |
| `testQualityAssuranceAgent` | 测试质量 | 测试用例设计、全类型测试、bug 管理 |
| `engineeringDevOpsAgent` | 工程化 | 工程化体系、CI/CD 流水线 |

### 开发流程

本项目支持三种开发流程，根据需求文档的完整程度选择合适的流程：

#### 1. 完整流程 (docs 目录含需求文档 + UI 设计图)
- 需求解析 → 设计输出 → 架构评审 → 核心/业务开发 → 测试验收 → 工程化部署 → 汇总验收

#### 2. 无 UI 设计图 (仅文字需求文档)
- 摘除 experienceVisualDesignAgent
- requirementAnalysisAgent 补充交互描述
- techArchitectureAgent 制定基础视觉规范

#### 3. 简单功能调整 (无指定需求文档)
- 摘除需求分析与设计 Agent
- coreCoordinatorAgent 直接拆解任务
- 快速开发与回归测试

---

### 任务拆分与验收规则

在核心/业务开发阶段，采用小任务闭环开发模式：

1. **任务拆分**：coreCoordinatorAgent 将开发任务拆分为若干小功能任务
2. **拆分记录**：拆分结果记录到 `docs/DevProcess/` 目录（如 `{任务名}-任务拆分.md`）
3. **任务交接**：coreCoordinatorAgent 调度开发 agent 开始执行第一个小任务
4. **上下文管理**：开发 agent 收到任务后，必须先查阅 `docs/DevProcess/` 中的开发进度文档
5. **开发→测试**：每个小功能任务完成后，进入测试验收环节
6. **验收结果**：
   - 验收不通过 → 退回开发环节修复
   - 验收通过 → 更新进度文档，继续执行下一个小任务
7. **全流程闭环**：所有小任务完成后，进入工程化部署阶段

---

### 开发过程文档管理

开发过程中产生的文档统一存放在 `docs/` 目录：

| 目录 | 用途 |
|------|------|
| `docs/DevDesign/` | 开发设计文档（技术方案、接口设计、交互流程说明等） |
| `docs/DevProcess/` | 开发过程文档（任务拆分结果、开发进度记录、问题日志等） |

**关键规则**：
- 每个总任务对应一个开发进度文档（如 `{任务名}-开发进度.md`）
- 每次完成小任务后更新进度文档
- 开启新任务前必须先查阅进度文档，确保开发连续性

### OpenCode 依赖

项目 `.opencode/package.json` 中已配置 OpenCode 插件：

```json
{
  "dependencies": {
    "@opencode-ai/plugin": "1.2.24"
  }
}
```

## 代码规范

### Vue 组件

- 使用 Composition API + `<script setup>` 语法
- 使用 `defineProps` / `defineEmits` 定义属性和事件
- 推荐使用 TypeScript

### 文件命名

- **组件**: PascalCase (`UserProfile.vue`)
- **组合式函数**: camelCase，以 `use` 开头 (`useAuth.ts`)
- **工具函数**: camelCase (`formatDate.ts`)
- **常量**: SCREAMING_SNAKE_CASE (`API_ENDPOINTS.ts`)

### 样式

项目已集成 Less，直接在组件中使用：

```vue
<style lang="less">
@primary-color: #409eff;

.component {
  color: @primary-color;
}
</style>
```

## 许可证

MIT
