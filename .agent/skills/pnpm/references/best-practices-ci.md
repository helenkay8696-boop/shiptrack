---
name: pnpm-ci-cd-setup
description: Optimizing pnpm for continuous integration and deployment workflows
---

# pnpm CI/CD Setup

## GitHub Actions

```yaml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v4
        with:
          version: 9
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'pnpm'
      - run: pnpm install --frozen-lockfile
      - run: pnpm test
      - run: pnpm build
```

## Key CI Flags

- `--frozen-lockfile` - Always use in CI, fails if lockfile needs updates
- `--prefer-offline` - Use cached packages when available
- `--ignore-scripts` - Skip lifecycle scripts for faster installs

## Corepack Integration

```json
{ "packageManager": "pnpm@9.0.0" }
```

## Monorepo: Build Changed Only

```bash
pnpm --filter "...[origin/main]" build
```

## Best Practices

1. Always use `--frozen-lockfile` in CI
2. Cache the pnpm store for faster installs
3. Use Corepack for consistent pnpm versions
4. Use `--filter` in monorepos for changed packages only
