---
name: vite-features
description: Vite core features including glob imports, env variables, and HMR
---

# Vite Core Features

## Glob Imports

Import multiple modules matching a pattern:

```ts
const modules = import.meta.glob('./dir/*.ts')
// { './dir/foo.ts': () => import('./dir/foo.ts'), ... }

// Eager loading
const modules = import.meta.glob('./dir/*.ts', { eager: true })
// { './dir/foo.ts': Module, ... }
```

## Asset Queries

```ts
import rawText from './file.txt?raw'     // as string
import assetUrl from './file.png?url'    // resolved URL
import worker from './worker.js?worker'  // as Web Worker
```

## Environment Variables

Access via `import.meta.env`:

```ts
import.meta.env.MODE       // 'development' | 'production'
import.meta.env.BASE_URL   // base public path
import.meta.env.DEV        // boolean
import.meta.env.PROD       // boolean
import.meta.env.SSR        // boolean
```

Custom env vars from `.env` files (must start with `VITE_`):

```bash
# .env
VITE_API_URL=https://api.example.com
```

```ts
import.meta.env.VITE_API_URL
```

## HMR API

```ts
if (import.meta.hot) {
  import.meta.hot.accept((newModule) => {
    // Handle updated module
  })
  
  import.meta.hot.dispose((data) => {
    // Cleanup before module is replaced
  })
}
```

<!--
Source references:
- https://vite.dev/guide/features
-->
