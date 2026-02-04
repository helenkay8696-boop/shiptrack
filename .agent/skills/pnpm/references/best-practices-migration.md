---
name: migration-to-pnpm
description: Migrating from npm or Yarn to pnpm with minimal friction
---

# Migration to pnpm

## Quick Migration

### From npm
```bash
rm -rf node_modules package-lock.json
pnpm install
```

### From Yarn
```bash
rm -rf node_modules yarn.lock
pnpm install
```

### Import Existing Lockfile
```bash
pnpm import  # Creates pnpm-lock.yaml from package-lock.json or yarn.lock
```

## Handling Common Issues

### Phantom Dependencies
pnpm is strict. Add missing dependencies explicitly:
```bash
pnpm add lodash  # If code imports lodash but it's not in package.json
```

### Missing Peer Dependencies
```ini
# .npmrc
auto-install-peers=true
```

### Symlink Issues
```ini
# .npmrc - Use hoisted mode for compatibility
node-linker=hoisted
```

## Monorepo Migration

1. Create `pnpm-workspace.yaml`:
```yaml
packages:
  - 'packages/*'
```

2. Update internal deps to workspace protocol:
```json
{ "@myorg/utils": "workspace:^" }
```

## CI/CD Migration

```yaml
# Before (npm)
- run: npm ci

# After (pnpm)
- uses: pnpm/action-setup@v4
- run: pnpm install --frozen-lockfile
```

Add to package.json:
```json
{ "packageManager": "pnpm@9.0.0" }
```
