---
name: server-side-rendering
description: SSR setup, state hydration, and avoiding cross-request state pollution
---

# Server Side Rendering (SSR)

Pinia works with SSR when stores are called at the top of `setup`, getters, or actions.

## Basic Usage

```vue
<script setup>
// ✅ Works - pinia knows the app context in setup
const main = useMainStore()
</script>
```

## Using Store Outside setup()

Pass the `pinia` instance explicitly:

```ts
router.beforeEach((to) => {
  // ✅ Pass pinia for correct SSR context
  const main = useMainStore(pinia)

  if (to.meta.requiresAuth && !main.isLoggedIn) {
    return '/login'
  }
})
```

## State Hydration

### Server Side

```ts
import devalue from 'devalue'

const serializedState = devalue(pinia.state.value)
// Inject into HTML as global variable
```

### Client Side

```ts
if (typeof window !== 'undefined') {
  pinia.state.value = JSON.parse(window.__pinia)
}
```

## Key Points

1. Call stores inside functions, not at module scope
2. Pass `pinia` instance when using stores outside components in SSR
3. Hydrate state before calling any `useStore()`
4. Use `devalue` or similar for safe serialization
