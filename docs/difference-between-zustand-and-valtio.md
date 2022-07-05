# What's the difference between zustand and valtio?

Ref: https://github.com/pmndrs/zustand/issues/483

## The major difference is: zustand is immutable state model, whereas valtio is mutable state model.

```js
import create from 'zustand'

// intentionally show non-hook deep usage
const store = create(() => ({ obj: { count: 0 } }))
store.setState((prev) => ({ obj: { count: prev.obj.count + 1 } })
```

```js
import { proxy } from 'valtio'

const state = proxy({ obj: { count: 0 } })
state.obj.count += 1
```

## Another difference is render optimization.

```js
import create from 'zustand'

const useStore = create(() => ({
  count1: 0,
  count2: 1,
}))

const Component = () => {
  const count1 = useStore((state) => state.count1) // manual render optimization with selector
  // ...
}
```

```js
import { proxy, useSnapshot } from 'valtio'

const state = proxy({
  count1: 0,
  count2: 0,
})

const Component = () => {
  const { count1 } = useSnapshot(state) // automatic render optimization with property access
  // ...
}
```
