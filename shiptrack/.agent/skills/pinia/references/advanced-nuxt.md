---
name: nuxt-integration
description: Using Pinia with Nuxt - auto-imports, SSR, and best practices
---

# Nuxt Integration

Pinia works seamlessly with Nuxt 3/4, handling SSR, serialization, and XSS protection automatically.

## Installation

```bash
npx nuxi@latest module add pinia
```

## Configuration

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@pinia/nuxt'],
})
```

## Auto Imports

These are automatically available:
- `usePinia()` - get pinia instance
- `defineStore()` - define stores
- `storeToRefs()` - extract reactive refs
- `acceptHMRUpdate()` - HMR support

**All stores in `app/stores/` (Nuxt 4) or `stores/` are auto-imported.**

## Fetching Data in Pages

```vue
<script setup>
const store = useStore()

// Run once, data persists across navigations
await callOnce('user', () => store.fetchUser())
</script>
```

## Using Stores in Middleware

```ts
// middleware/auth.ts
export default defineNuxtRouteMiddleware((to) => {
  const nuxtApp = useNuxtApp()
  const store = useStore(nuxtApp.$pinia)

  if (to.meta.requiresAuth && !store.isLoggedIn) {
    return navigateTo('/login')
  }
})
```

## Pinia Plugins with Nuxt

```ts
// plugins/myPiniaPlugin.ts
export default defineNuxtPlugin(({ $pinia }) => {
  $pinia.use(MyPiniaPlugin)
})
```
