---
name: plugins
description: Extend stores with custom properties, methods, and behavior
---

# Plugins

Plugins extend all stores with custom properties, methods, or behavior.

## Basic Plugin

```ts
import { createPinia } from 'pinia'

function SecretPiniaPlugin() {
  return { secret: 'the cake is a lie' }
}

const pinia = createPinia()
pinia.use(SecretPiniaPlugin)

// In any store
const store = useStore()
store.secret // 'the cake is a lie'
```

## Plugin Context

```ts
import { PiniaPluginContext } from 'pinia'

export function myPiniaPlugin(context: PiniaPluginContext) {
  context.pinia   // pinia instance
  context.app     // Vue app instance
  context.store   // store being augmented
  context.options // store definition options
}
```

## Adding State

Add to both `store` and `store.$state` for SSR/devtools:

```ts
import { toRef, ref } from 'vue'

pinia.use(({ store }) => {
  if (!store.$state.hasOwnProperty('hasError')) {
    const hasError = ref(false)
    store.$state.hasError = hasError
  }
  store.hasError = toRef(store.$state, 'hasError')
})
```

## Adding External Properties

Wrap non-reactive objects with `markRaw()`:

```ts
import { markRaw } from 'vue'
import { router } from './router'

pinia.use(({ store }) => {
  store.router = markRaw(router)
})
```

## TypeScript Augmentation

```ts
import 'pinia'
import type { Router } from 'vue-router'

declare module 'pinia' {
  export interface PiniaCustomProperties {
    router: Router
  }
}
```

## Nuxt Plugin

```ts
// plugins/myPiniaPlugin.ts
import { PiniaPluginContext } from 'pinia'

function MyPiniaPlugin({ store }: PiniaPluginContext) {
  store.$subscribe((mutation) => {
    console.log(`[🍍 ${mutation.storeId}]: ${mutation.type}`)
  })
  return { creationTime: new Date() }
}

export default defineNuxtPlugin(({ $pinia }) => {
  $pinia.use(MyPiniaPlugin)
})
```
