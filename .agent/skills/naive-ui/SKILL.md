---
name: naive-ui
description: Naive UI Vue 3 组件库。用于构建常见的 UI 组件如表格、表单、弹窗、抽屉等。
---

# Naive UI

Naive UI 是一个 Vue 3 组件库，完全使用 TypeScript 编写，主题可定制，提供 80+ 组件。

## 组件参考

| 类别 | 组件 | 参考 |
|------|------|------|
| 反馈 | Drawer 抽屉 | [drawer](references/drawer.md) |

## 安装

```bash
pnpm add naive-ui
```

## 基本使用

```ts
// main.ts
import { createApp } from 'vue'
import naive from 'naive-ui'

const app = createApp(App)
app.use(naive)
```

## 按需引入（推荐）

```vue
<script setup>
import { NButton, NDrawer, NDrawerContent } from 'naive-ui'
</script>

<template>
  <n-button>按钮</n-button>
</template>
```

## 配置 Provider

```vue
<template>
  <n-config-provider :theme="darkTheme">
    <n-message-provider>
      <n-dialog-provider>
        <App />
      </n-dialog-provider>
    </n-message-provider>
  </n-config-provider>
</template>
```
