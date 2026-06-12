# Rule 2 — Extract components

**Trigger:** a JSX block inside a component that:
- repeats (same structure rendered 2+ times, possibly with different data), OR
- has its own clear responsibility (a card, a row, a form section, a tooltip), OR
- makes the parent component hard to scan (parent > ~80 lines of JSX)

**Action:** extract to a named component in the same file or a new file. Props must be explicit — no "pass the whole object and destructure inside." Name it after what it *is*, not what it *does* (`UserCard`, not `renderUser`).

## Example

```tsx
// Before — repeated structure inline
{users.map(user => (
  <div className="flex gap-2 p-4 border rounded">
    <img src={user.avatar} />
    <span>{user.name}</span>
  </div>
))}

// After — extracted component
function UserRow({ avatar, name }: { avatar: string; name: string }) {
  return (
    <div className="flex gap-2 p-4 border rounded">
      <img src={avatar} />
      <span>{name}</span>
    </div>
  )
}

{users.map(user => <UserRow key={user.id} avatar={user.avatar} name={user.name} />)}
```

## Do not extract if

- The JSX is trivial (2–3 elements) and only appears once.
