# 代码风格指南

本文档包含项目的代码风格规范。当您编写或修改代码时，**必须**加载此文件并遵循其中的规范。

## Vue 组件

- 使用 Composition API + `<script setup>` 语法
- 使用 `defineProps` 定义属性，`defineEmits` 定义事件
- 尽量使用 TypeScript

## 文件命名

- **组件**: PascalCase (`UserProfile.vue`)
- **组合式函数**: camelCase，以 `use` 开头 (`useAuth.js`)
- **工具函数**: camelCase (`formatDate.js`)
- **常量**: SCREAMING_SNAKE_CASE (`API_ENDPOINTS.js`)

## 导入顺序

1. Vue 核心
2. 外部库（Element Plus、Vue Router、Pinia 等）
3. 内部模块（使用 `@/` 别名）
4. 资源文件

组间用空行分隔。

## TypeScript

- 新文件使用 `.ts` 或 `<script lang="ts">`
- 为属性和 API 响应定义接口
- 避免使用 `any`，不确定时使用 `unknown`

## 状态管理（Pinia）

- 在 `src/stores/` 目录创建 store
- 使用组合式 store 语法（函数形式）
- store 名称使用单数名词

## 路由

- 在 `src/router/` 定义路由
- 使用懒加载: `component: () => import('./views/Home.vue')`

## 样式（Less）

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

## 错误处理

- 异步操作使用 try-catch 包裹
- 通过 ElMessage 显示用户友好的错误信息
- 记录错误用于调试

## 最佳实践

1. 保持组件小巧 —— 单一职责
2. 将可复用逻辑提取为组合式函数
3. 避免属性穿透 —— 使用 provide/inject 或 Pinia
4. 使用 `v-memo`、`shallowRef` 优化性能
5. 添加 ARIA 属性提升可访问性
6. 过滤用户输入确保安全

## 提交前检查

1. 修改逻辑相关代码或配置后，执行 `/init` 指令同步更新 AGENTS.md
2. 运行 `npm run build` —— 无错误
3. 检查控制台错误
4. 测试错误状态和加载状态
