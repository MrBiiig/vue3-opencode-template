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

> **原则**：所有流程需遵循「职责闭环、质量可控」原则，Agent 仅执行对应核心职责，无冗余操作。

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

docs/               # 需求文档目录
.opencode/
└── agents/         # Agent 角色配置
```

## 构建 / 开发命令

```bash
npm run dev      # 启动开发服务器
npm run build    # 构建生产版本
npm run preview  # 预览生产版本
```

### 测试（未配置）

```bash
npm install -D vitest @vue/test-utils jsdom
npm run test              # 运行所有测试
npm run test -- filename.spec.js  # 运行单个测试文件
npm run test -- --watch   # 监听模式
npm run test -- --coverage  # 覆盖率
```

### 代码检查（未配置）

```bash
npm install -D eslint @eslint/js eslint-plugin-vue
npm run lint     # 运行代码检查
npm run lint -- --fix  # 自动修复
```

## 代码风格指南

### Vue 组件

- 使用 Composition API + `<script setup>` 语法
- 使用 `defineProps` 定义属性，`defineEmits` 定义事件
- 尽量使用 TypeScript

### 文件命名

- **组件**: PascalCase (`UserProfile.vue`)
- **组合式函数**: camelCase，以 `use` 开头 (`useAuth.js`)
- **工具函数**: camelCase (`formatDate.js`)
- **常量**: SCREAMING_SNAKE_CASE (`API_ENDPOINTS.js`)

### 导入顺序

1. Vue 核心
2. 外部库（Element Plus、Vue Router、Pinia 等）
3. 内部模块（使用 `@/` 别名）
4. 资源文件

组间用空行分隔。

### TypeScript

- 新文件使用 `.ts` 或 `<script lang="ts">`
- 为属性和 API 响应定义接口
- 避免使用 `any`，不确定时使用 `unknown`

### 状态管理（Pinia）

- 在 `src/stores/` 目录创建 store
- 使用组合式 store 语法（函数形式）
- store 名称使用单数名词

### 路由

- 在 `src/router/` 定义路由
- 使用懒加载: `component: () => import('./views/Home.vue')`

### 样式（Less）

项目已集成 Less，在 Vue 组件中使用 `<style lang="less">` 即可：

```vue
<style lang="less">
// 变量
@primary-color: #409eff;

// 混入
.border-radius(@radius: 4px) {
  border-radius: @radius;
}

.component {
  color: @primary-color;
  
  .child {
    .border-radius();
  }
}
</style>
```

### 错误处理

- 异步操作使用 try-catch 包裹
- 通过 ElMessage 显示用户友好的错误信息
- 记录错误用于调试

### 最佳实践

1. 保持组件小巧 —— 单一职责
2. 将可复用逻辑提取为组合式函数
3. 避免属性穿透 —— 使用 provide/inject 或 Pinia
4. 使用 `v-memo`、`shallowRef` 优化性能
5. 添加 ARIA 属性提升可访问性
6. 过滤用户输入确保安全

### 提交前

1. 修改逻辑相关代码或配置后，执行 `/init` 指令同步更新 AGENTS.md
2. 运行 `npm run build` —— 无错误
3. 检查控制台错误
4. 测试错误状态和加载状态
