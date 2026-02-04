---
name: testing
description: Unit testing stores and components with @pinia/testing
---

# Testing Stores

## Unit Testing Stores

Create a fresh pinia instance for each test:

```ts
import { setActivePinia, createPinia } from 'pinia'
import { useCounterStore } from '../src/stores/counter'

describe('Counter Store', () => {
  beforeEach(() => {
    setActivePinia(createPinia())
  })

  it('increments', () => {
    const counter = useCounterStore()
    expect(counter.n).toBe(0)
    counter.increment()
    expect(counter.n).toBe(1)
  })
})
```

## Testing Components

Use `createTestingPinia()`:

```ts
import { mount } from '@vue/test-utils'
import { createTestingPinia } from '@pinia/testing'
import { useSomeStore } from '@/stores/myStore'

const wrapper = mount(Counter, {
  global: {
    plugins: [createTestingPinia()],
  },
})

const store = useSomeStore()

// Manipulate state directly
store.name = 'new name'

// Actions are stubbed by default
store.someAction()
expect(store.someAction).toHaveBeenCalledTimes(1)
```

## Initial State

```ts
createTestingPinia({
  initialState: {
    counter: { n: 20 }, // Store name → initial state
  },
})
```

## Action Stubbing

```ts
// Execute real actions
createTestingPinia({ stubActions: false })

// Mock return values
store.someAction.mockResolvedValue('mocked value')
```

## Mocking Getters

Getters are writable in tests:

```ts
const counter = useCounterStore(pinia)
counter.double = 3 // Override computed value
```
