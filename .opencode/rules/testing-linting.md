# 测试与代码检查指南

本文档包含项目的测试和代码检查配置。当前项目**尚未配置**测试和代码检查工具，仅作为未来扩展的参考指南。

当您需要配置测试或代码检查功能时，**必须**加载此文件。

---

## 测试（未配置）

项目当前未安装测试框架，以下是建议的安装和配置方案：

### 安装依赖

```bash
npm install -D vitest @vue/test-utils jsdom
```

### 可用命令

```bash
npm run test              # 运行所有测试
npm run test -- filename.spec.js  # 运行单个测试文件
npm run test -- --watch   # 监听模式
npm run test -- --coverage  # 覆盖率
```

### 测试配置建议

在 `vite.config.ts` 中添加测试配置:

```typescript
import { defineConfig } from 'vitest/config'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  test: {
    environment: 'jsdom',
    include: ['src/**/*.{test,spec}.{js,ts}'],
  },
  plugins: [vue()],
})
```

---

## 代码检查（未配置）

项目当前未安装 ESLint，以下是建议的安装和配置方案：

### 安装依赖

```bash
npm install -D eslint @eslint/js eslint-plugin-vue
```

### 可用命令

```bash
npm run lint     # 运行代码检查
npm run lint -- --fix  # 自动修复
```

### ESLint 配置建议

创建 `.eslintrc.cjs`:

```javascript
/* eslint-env node */
require('@eslint/js')

module.exports = {
  extends: [
    'eslint:recommended',
    'plugin:vue/vue3-recommended',
  ],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  rules: {
    // 自定义规则
  },
}
```
