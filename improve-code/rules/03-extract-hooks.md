# Rule 3 — Extract custom hooks

**Trigger:** logic inside a component that:
- mixes data-fetching + state + effects, OR
- is reused (same `useEffect`/`useState` combo in 2+ components), OR
- could be named `use<Something>` and make the component easier to read

**Action:** move the logic to `hooks/use<Name>.ts`. The hook returns only what callers need — not the full internal state. The component becomes a thin consumer.

## Example

```tsx
// Before — data fetching mixed into the component
function UserList() {
  const [users, setUsers] = useState([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    fetchUsers().then(data => {
      setUsers(data)
      setLoading(false)
    })
  }, [])

  if (loading) return <Spinner />
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>
}

// After — hook owns the fetching logic
function useUsers() {
  const [users, setUsers] = useState([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    fetchUsers().then(data => {
      setUsers(data)
      setLoading(false)
    })
  }, [])

  return { users, loading }
}

function UserList() {
  const { users, loading } = useUsers()
  if (loading) return <Spinner />
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>
}
```

## Common patterns worth extracting

- Data fetching with loading/error state
- Debounced input
- Form field binding
- Intersection observer
- Polling

## Do not extract if

- The hook would be a one-liner.
- It's only ever used once and has no reuse potential.
