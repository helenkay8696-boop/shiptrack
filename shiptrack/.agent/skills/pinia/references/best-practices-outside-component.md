---
name: using-stores-outside-components
description: Correctly using stores in navigation guards, plugins, and other non-component contexts
---

# Using Stores Outside Components

Stores need the `pinia` instance, which is automatically injected in components. Outside components, you may need to provide it manually.

## Single Page Applications

Call stores **after** pinia is installed:

```ts
const pinia = createPinia()
const app = createApp(App)
app.use(pinia)

// ✅ Works - pinia is active
const userStore = useUserStore()
```

## Navigation Guards

**Wrong:** Call at module level

```ts
// ❌ May fail depending on import order
const store = useUserStore()

router.beforeEach((to) => {
  if (store.isLoggedIn) { /* ... */ }
})
```

**Correct:** Call inside the guard

```ts
router.beforeEach((to) => {
  // ✅ Called after pinia is installed
  const store = useUserStore()

  if (to.meta.requiresAuth && !store.isLoggedIn) {
    return '/login'
  }
})
```

## SSR Applications

Always pass the `pinia` instance to `useStore()`:

```ts
router.beforeEach((to) => {
  // ✅ Pass pinia instance
  const main = useMainStore(pinia)

  if (to.meta.requiresAuth && !main.isLoggedIn) {
    return '/login'
  }
})
```

## Key Takeaway

Defer `useStore()` calls to functions that run after pinia is installed, rather than calling at module scope.
