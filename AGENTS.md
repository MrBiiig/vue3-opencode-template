# AGENTS.md - Agent 编码指南

本文档为在本项目中工作的 AI Agent 提供编码指南。

## 语言提示

> **重要提示**：当前项目中提问时，除非用户提示词特别指定其他语言，否则生成描述性内容时，描述的主语言使用中文。

## 项目概览

- **项目类型**: Vue 3 + Vite SPA
- **框架**: Vue 3 (Composition API + `<script setup>`)
- **状态管理**: Pinia
- **路由**: Vue Router
- **UI 库**: Element Plus

## OpenCode Agent 配置

项目已配置 8 个角色的 Agent 定义文件，位于 `.opencode/agents/` 目录：

| Agent 文件 | 角色 |
|-----------|------|
| `coreCoordinatorAgent.md` | 核心协调 Agent |
| `requirementAnalysisAgent.md` | 需求分析与管理 Agent |
| `experienceVisualDesignAgent.md` | 体验与视觉设计 Agent |
| `techArchitectureAgent.md` | 技术架构 Agent |
| `coreDevelopmentAgent.md` | 核心开发 Agent |
| `businessDevelopmentAgent.md` | 业务开发 Agent |
| `testQualityAssuranceAgent.md` | 测试与质量保障 Agent |
| `engineeringDevOpsAgent.md` | 工程化与 DevOps Agent |

每个 Agent 都有其专属的系统提示词配置，执行特定任务时会自动调用对应角色。

## 多 Agent 开发流程规则

### 1. 完整开发流程（docs目录含需求文档+UI设计图）

**流程闭环**：需求解析 → 设计输出 → 架构评审 → 核心/业务开发 → 测试验收 → 工程化部署 → 核心协调 Agent 汇总验收

### 2. 无 UI 设计图开发流程（docs目录仅文字需求文档）

摘除 experienceVisualDesignAgent，调整流程：
- requirementAnalysisAgent 补充交互逻辑文字描述
- techArchitectureAgent 新增「基础视觉规范默认值」制定
- coreDevelopmentAgent / businessDevelopmentAgent 按通用视觉规范开发

### 3. 简单功能调整流程（无指定需求文档，仅口头/文字功能指令）

摘除 requirementAnalysisAgent / experienceVisualDesignAgent，调整流程：
- coreCoordinatorAgent 直接拆解功能调整任务
- techArchitectureAgent 快速评审技术可行性
- 核心开发/业务开发 Agent 按需分工实现
- testQualityAssuranceAgent 仅执行功能回归测试
- engineeringDevOpsAgent 仅执行增量构建/部署

### 任务执行与验收规则

在核心/业务开发阶段，需将开发任务拆分为若干小功能任务，逐一执行验收：

- **任务拆分**：由 coreCoordinatorAgent 负责将开发任务拆分为一个一个小功能任务
- **拆分结果记录**：将拆分结果记录到项目 `docs/DevProcess/` 目录中（如 `{任务名}-任务拆分.md`），供后续开发参考
- **开发→测试**：每个小功能任务完成后，进入测试验收环节
- **验收结果处理**：
  - 验收不通过 → 退回开发环节进行修复
  - 验收通过 → 闭环当前任务，继续执行下一个小功能任务
- **全流程闭环**：所有小任务完成后，进入工程化部署阶段

### 开发过程记录规则

在核心/业务开发阶段，需全程记录开发进度与问题：

- **开发进度维护**：每次完成一个小的拆分功能任务后，在 `docs/DevProcess/` 目录中维护开发进度（如 `{任务名}-开发进度.md`），记录当前已完成任务、进行中任务、待处理任务
- **问题记录**：同步记录开发过程中遇到的问题及解决方案，供后续开发参考
- **上下文管理**：每个小任务完成后清空当前上下文，开启下一个小任务时先查阅开发进度文档，确保连续性
- **文档命名**：每个总任务对应一个开发进度文档，无需每个小任务单独创建文档

### 评审流程规则

各环节评审遵循「输出-评审-调整-确认」闭环：
- 由输出方 Agent 发起评审，邀请相关 Agent 参与
- 若有评审意见，输出方 Agent 据此调整输出物后重新提交评审
- 直至无需调整，方可进入下一环节

> **原则**：所有流程需遵循「职责闭环、质量可控」原则，Agent 仅执行对应核心职责，无冗余操作。

### 开发设计文档管理

根据需求产生的开发设计文档（如技术方案、接口设计、数据库设计、交互流程说明等）统一存放在项目根目录下的 `docs/DevDesign/` 目录中，供后续其他 Agent 评审和参考。

### 开发过程文档管理

核心/业务开发阶段产生的过程文档（如任务拆分结果、开发进度记录、问题日志等）统一存放在项目根目录下的 `docs/DevProcess/` 目录中，供后续开发连续性参考。

## 项目结构

```
src/
├── components/     # Vue 组件
├── views/          # 页面视图
├── stores/         # Pinia 状态管理
├── router/         # 路由配置
├── utils/          # 工具函数
├── api/            # API 接口
└── assets/         # 静态资源

docs/               # 文档目录
├── DevDesign/      # 开发设计文档（技术方案、接口设计等）
└── DevProcess/     # 开发过程文档（任务拆分、进度记录、问题日志）
.opencode/
├── agents/         # Agent 角色配置
└── rules/          # 规则文件（按需加载）
```

---

## 外部文件加载规则

**重要提示**：以下文件引用采用懒加载模式，请根据实际任务需要加载对应文件，**不要 preemptively 加载所有引用**。

### 代码风格指南

当您编写或修改代码时，请立即加载以下文件作为强制规范：

@.opencode/rules/code-style.md - 代码风格指南

### 开发命令参考

当您需要执行构建、启动开发服务器或运行脚本时，请加载：

@.opencode/rules/dev-commands.md - 构建与开发命令

### 测试与代码检查

当您需要配置测试、代码检查功能，或执行相关任务时，请加载：

@.opencode/rules/testing-linting.md - 测试与代码检查指南（当前未配置，仅作未来参考）
