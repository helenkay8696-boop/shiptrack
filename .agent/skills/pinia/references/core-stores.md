---
name: stores
description: Defining stores, state, getters, and actions in Pinia
---

# Pinia Stores

Stores are defined using `defineStore()` with a unique name. Each store has three core concepts: **state**, **getters**, and **actions**.

## Defining Stores

### Option Stores

```ts
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0,
    name: 'Eduardo',
  }),
  getters: {
    doubleCount: (state) => state.count * 2,
  },
  actions: {
    increment() {
      this.count++
    },
  },
})
```

### Setup Stores (Recommended)

```ts
import { ref, computed } from 'vue'
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const name = ref('Eduardo')
  const doubleCount = computed(() => count.value * 2)

  function increment() {
    count.value++
  }

  return { count, name, doubleCount, increment }
})
```

In Setup Stores: `ref()` → state, `computed()` → getters, `function()` → actions.

### Using Stores

```vue
<script setup>
import { useCounterStore } from '@/stores/counter'

const store = useCounterStore()
// Access: store.count, store.doubleCount, store.increment()
</script>
```

### Destructuring with storeToRefs

```ts
import { storeToRefs } from 'pinia'

const store = useCounterStore()

// ❌ Breaks reactivity
const { name, doubleCount } = store

// ✅ Preserves reactivity for state/getters
const { name, doubleCount } = storeToRefs(store)

// ✅ Actions can be destructured directly
const { increment } = store
```

## State

### Mutating with $patch

```ts
// Object syntax
store.$patch({
  count: store.count + 1,
  name: 'DIO',
})

// Function syntax (for complex mutations)
store.$patch((state) => {
  state.items.push({ name: 'shoes', quantity: 1 })
  state.hasChanged = true
})
```

### Subscribing to State Changes

```ts
cartStore.$subscribe((mutation, state) => {
  mutation.type // 'direct' | 'patch object' | 'patch function'
  mutation.storeId // 'cart'
  localStorage.setItem('cart', JSON.stringify(state))
})
```

## Getters

```ts
getters: {
  doubleCount: (state) => state.count * 2,
  // Access other getters with this
  doublePlusOne(): number {
    return this.doubleCount + 1
  },
  // Getter with arguments (loses caching)
  getUserById: (state) => {
    return (userId: string) => state.users.find((user) => user.id === userId)
  },
},
```

## Actions

```ts
actions: {
  increment() {
    this.count++
  },
  async registerUser(login: string, password: string) {
    this.userData = await api.post({ login, password })
  },
},
```

### Subscribing to Actions

```ts
someStore.$onAction(({ name, args, after, onError }) => {
  after((result) => {
    console.log(`Finished "${name}"`)
  })
  onError((error) => {
    console.warn(`Failed "${name}": ${error}`)
  })
})
```

## Options API Helpers

```ts
import { mapState, mapWritableState, mapActions } from 'pinia'

export default {
  computed: {
    ...mapState(useCounterStore, ['count', 'doubleCount']),
    ...mapWritableState(useCounterStore, ['count']),
  },
  methods: {
    ...mapActions(useCounterStore, ['increment']),
  },
}
```
