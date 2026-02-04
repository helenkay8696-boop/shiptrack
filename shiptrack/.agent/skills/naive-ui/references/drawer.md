---
name: naive-ui-drawer
description: Naive UI Drawer 抽屉组件：从屏幕边缘滑出的面板，用于展示详情或表单
---

# Naive UI Drawer 组件

## 基本用法

```vue
<template>
  <n-button @click="active = true">打开抽屉</n-button>
  <n-drawer v-model:show="active" :width="502">
    <n-drawer-content title="抽屉标题">
      抽屉内容...
    </n-drawer-content>
  </n-drawer>
</template>

<script setup>
import { ref } from 'vue'

const active = ref(false)
</script>
```

## 主要 Props

### n-drawer

| 属性 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| show | `boolean` | `false` | 是否显示抽屉 |
| placement | `'top' \| 'right' \| 'bottom' \| 'left'` | `'right'` | 抽屉弹出位置 |
| width | `number \| string` | `251` | 抽屉宽度（placement 为 left/right 时生效） |
| height | `number \| string` | `251` | 抽屉高度（placement 为 top/bottom 时生效） |
| closable | `boolean` | `true` | 是否显示关闭按钮 |
| mask-closable | `boolean` | `true` | 点击遮罩是否关闭 |
| to | `string \| HTMLElement` | `'body'` | 抽屉挂载位置 |

## DrawerContent 组件

`<n-drawer-content>` 用于包裹抽屉内容，提供标题、页脚等结构：

```vue
<n-drawer-content title="标题" closable>
  <template #header>自定义头部</template>
  <template #footer>
    <n-button>取消</n-button>
    <n-button type="primary">确定</n-button>
  </template>
  主体内容...
</n-drawer-content>
```

### DrawerContent Props

| 属性 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| title | `string` | - | 标题 |
| closable | `boolean` | `true` | 是否显示关闭按钮 |
| native-scrollbar | `boolean` | `false` | 是否使用原生滚动条 |
| body-content-class | `string` | - | 主体区域 class |
| footer-class | `string` | - | 页脚区域 class |

### DrawerContent Slots

| 插槽 | 说明 |
|------|------|
| default | 主体内容 |
| header | 自定义头部 |
| footer | 自定义页脚 |

## 不同位置

```vue
<n-drawer v-model:show="show" placement="left" :width="300">
  <n-drawer-content title="左侧抽屉">内容</n-drawer-content>
</n-drawer>

<n-drawer v-model:show="show" placement="top" :height="200">
  <n-drawer-content title="顶部抽屉">内容</n-drawer-content>
</n-drawer>
```

## 禁用遮罩关闭

```vue
<n-drawer v-model:show="show" :mask-closable="false">
  <n-drawer-content title="点击遮罩不关闭">
    只能通过关闭按钮关闭
  </n-drawer-content>
</n-drawer>
```
