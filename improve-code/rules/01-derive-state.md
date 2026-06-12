# Rule 1 — Eliminate useless useState

**Trigger:** a `useState` whose value is always derivable from props, other state, or a selector without side effects.

**Action:** delete the state variable. Compute the value inline or with `useMemo` if the computation is expensive. Never keep state around "for performance" without measuring.

## Examples of useless state

```tsx
// Bad — fullName is always derivable
const [fullName, setFullName] = useState(first + ' ' + last)

// Good — derive it directly
const fullName = first + ' ' + last
```

```tsx
// Bad — isLoading mirrors another state value
const [isLoading, setIsLoading] = useState(items.length === 0)

// Good — derive it
const isLoading = items.length === 0
```

Other common cases: filtered/sorted lists that recompute from another state array, booleans that track whether a list is empty, computed totals.

## When state IS justified

- User input (controlled inputs)
- Server responses not managed by a data-fetching library
- Toggle/modal open state
- Optimistic UI updates
