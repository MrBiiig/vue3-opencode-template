# 开发命令参考

本文档包含项目的构建和开发命令。当您需要执行构建、启动开发服务器或运行脚本时，**必须**加载此文件。

## 构建 / 开发命令

```bash
npm run dev      # 启动开发服务器
npm run build    # 构建生产版本
npm run preview  # 预览生产版本
```

## 常用开发操作

### 安装依赖

```bash
npm install      # 安装项目依赖
```

### 类型检查

项目当前未配置完整的 TypeScript 检查，如需检查可使用:

```bash
npx vue-tsc --noEmit  # 类型检查（如果已配置）
```
