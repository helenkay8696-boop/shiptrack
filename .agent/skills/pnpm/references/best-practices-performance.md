---
name: pnpm-performance-optimization
description: Tips and tricks for faster installs and better performance
---

# pnpm Performance Optimization

## Install Optimizations

```bash
# Frozen lockfile (fastest, skips resolution)
pnpm install --frozen-lockfile

# Prefer offline (use cache)
pnpm install --prefer-offline

# Skip optional deps
pnpm install --no-optional

# Skip scripts
pnpm install --ignore-scripts
```

## Store Optimizations

```ini
# .npmrc
side-effects-cache=true  # Cache native module builds
store-dir=~/.pnpm-store  # Shared store (default)
```

```bash
pnpm store prune  # Remove unreferenced packages
```

## Workspace Optimizations

```bash
# Parallel execution
pnpm -r --parallel run build

# Build changed only
pnpm --filter "...[origin/main]" build

# Stream output
pnpm -r --stream run dev
```

## Configuration Summary

```ini
# .npmrc - Optimized for performance
prefer-offline=true
auto-install-peers=true
side-effects-cache=true
workspace-concurrency=4
```

## Quick Reference

| Scenario | Command |
|----------|---------|
| CI installs | `pnpm install --frozen-lockfile` |
| Changed packages only | `pnpm --filter "...[origin/main]" build` |
| Parallel workspace | `pnpm -r --parallel run build` |
| Clean store | `pnpm store prune` |
