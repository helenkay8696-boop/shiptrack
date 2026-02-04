---
name: advanced-patterns
description: Vue 3 built-in components (Transition, Teleport, Suspense, KeepAlive) and advanced directives
---

# Built-in Components & Directives

## Transition

```vue
<template>
  <Transition name="fade">
    <div v-if="show">Content</div>
  </Transition>
</template>

<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
```

### CSS Classes

| Class | When |
|-------|------|
| `{name}-enter-from` | Start state for enter |
| `{name}-enter-active` | Active state for enter |
| `{name}-enter-to` | End state for enter |
| `{name}-leave-from` | Start state for leave |
| `{name}-leave-active` | Active state for leave |
| `{name}-leave-to` | End state for leave |

### Transition Modes

```vue
<!-- Wait for leave to complete before enter -->
<Transition name="fade" mode="out-in">
  <component :is="currentView" />
</Transition>
```

## TransitionGroup

```vue
<template>
  <TransitionGroup name="list" tag="ul">
    <li v-for="item in items" :key="item.id">
      {{ item.text }}
    </li>
  </TransitionGroup>
</template>

<style>
.list-enter-active, .list-leave-active {
  transition: all 0.3s ease;
}
.list-enter-from, .list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
.list-move {
  transition: transform 0.3s ease;
}
</style>
```

## Teleport

```vue
<template>
  <button @click="open = true">Open Modal</button>
  
  <Teleport to="body">
    <div v-if="open" class="modal">
      Modal content rendered at body
    </div>
  </Teleport>
</template>
```

### Props

```vue
<Teleport to="#modal-container">
<Teleport :to="targetElement">
<Teleport to="body" :disabled="isMobile">
<Teleport defer to="#late-rendered-target">  <!-- Vue 3.5+ -->
```

## Suspense

**Experimental feature.**

```vue
<template>
  <Suspense>
    <template #default>
      <AsyncComponent />
    </template>
    <template #fallback>
      <div>Loading...</div>
    </template>
  </Suspense>
</template>
```

Suspense waits for:
- Components with `async setup()`
- Components using top-level `await` in `<script setup>`
- Async components created with `defineAsyncComponent`

## KeepAlive

```vue
<template>
  <KeepAlive>
    <component :is="currentTab" />
  </KeepAlive>
</template>
```

### Include/Exclude

```vue
<KeepAlive include="ComponentA,ComponentB">
<KeepAlive :include="/^Tab/">
<KeepAlive exclude="ModalComponent">
<KeepAlive :max="10">
```

### Lifecycle Hooks

```ts
import { onActivated, onDeactivated } from 'vue'

onActivated(() => {
  // Called when component is inserted from cache
})

onDeactivated(() => {
  // Called when component is removed to cache
})
```

## v-memo

Skip re-renders when dependencies unchanged.

```vue
<template>
  <div v-for="item in list" :key="item.id" v-memo="[item.selected]">
    <!-- Only re-renders when item.selected changes -->
    <ExpensiveComponent :item="item" />
  </div>
</template>
```

## Custom Directives

```ts
const vFocus: Directive<HTMLElement> = {
  mounted: (el) => el.focus()
}

const vColor: Directive<HTMLElement, string> = {
  mounted(el, binding) {
    el.style.color = binding.value
  },
  updated(el, binding) {
    el.style.color = binding.value
  },
}
```

### Arguments & Modifiers

```vue
<div v-color:background.bold="'red'">

<script setup lang="ts">
const vColor: Directive<HTMLElement, string> = {
  mounted(el, binding) {
    // binding.arg = 'background'
    // binding.modifiers = { bold: true }
    // binding.value = 'red'
    el.style[binding.arg || 'color'] = binding.value
    if (binding.modifiers.bold) {
      el.style.fontWeight = 'bold'
    }
  }
}
</script>
```
