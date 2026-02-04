---
name: script-setup-macros
description: Vue 3 script setup syntax and compiler macros for defining props, emits, models, and more
---

# Script Setup & Macros

`<script setup>` is the recommended syntax for Vue SFCs with Composition API. It provides better runtime performance and IDE type inference.

## Basic Syntax

```vue
<script setup lang="ts">
// Top-level bindings are exposed to template
import { ref } from 'vue'
import MyComponent from './MyComponent.vue'

const count = ref(0)
const increment = () => count.value++
</script>

<template>
  <button @click="increment">{{ count }}</button>
  <MyComponent />
</template>
```

## defineProps

```ts
// Type-based declaration (recommended)
const props = defineProps<{
  title: string
  count?: number
  items: string[]
}>()

// With defaults (Vue 3.5+)
const { title, count = 0 } = defineProps<{
  title: string
  count?: number
}>()

// With defaults (Vue 3.4 and below)
const props = withDefaults(defineProps<{
  title: string
  items?: string[]
}>(), {
  items: () => []  // Use factory for arrays/objects
})
```

## defineEmits

```ts
// Named tuple syntax (recommended)
const emit = defineEmits<{
  update: [value: string]
  change: [id: number, name: string]
  close: []
}>()

emit('update', 'new value')
emit('change', 1, 'name')
emit('close')
```

## defineModel

Two-way binding prop consumed via `v-model`. Available in Vue 3.4+.

```ts
// Basic usage - creates "modelValue" prop
const model = defineModel<string>()
model.value = 'hello'  // Emits "update:modelValue"

// Named model - consumed via v-model:name
const count = defineModel<number>('count', { default: 0 })

// With modifiers
const [value, modifiers] = defineModel<string>()
if (modifiers.trim) {
  // Handle trim modifier
}
```

## defineExpose

Explicitly expose properties to parent via template refs.

```ts
import { ref } from 'vue'

const count = ref(0)
const reset = () => { count.value = 0 }

defineExpose({ count, reset })
```

## defineOptions

```ts
defineOptions({
  inheritAttrs: false,
  name: 'CustomName'
})
```

## defineSlots

```ts
const slots = defineSlots<{
  default(props: { item: string; index: number }): any
  header(props: { title: string }): any
}>()
```

## Generic Components

```vue
<script setup lang="ts" generic="T extends string | number">
defineProps<{
  items: T[]
  selected: T
}>()
</script>
```

## Top-level await

Uses `await` directly. Component becomes async and must be used with `<Suspense>`.

```vue
<script setup lang="ts">
const data = await fetch('/api/data').then(r => r.json())
</script>
```
