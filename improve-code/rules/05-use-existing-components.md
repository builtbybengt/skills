# Rule 5 — Use existing components — never reinvent

**Trigger:** raw HTML elements (`<button>`, `<input>`, `<a>`, `<select>`, …) or inline JSX that duplicates a component that already exists in the project.

**Action:** before writing any new UI, scan the project for existing components. If one exists, use it.

## Scan step

Before reaching for a raw element, search for an existing component:

```bash
grep -r "export.*Button" src/
grep -r "export.*Input" src/
# etc.
```

Check `components/`, `ui/`, `shared/`, or wherever the project keeps its component library.

## Example

```tsx
// Before — raw element, ignores the project's Button
<button className="px-4 py-2 bg-blue-500 text-white rounded" onClick={fn}>
  Save
</button>

// After — uses the component that already exists
<Button onClick={fn}>Save</Button>
```

## Rules

- Never create a second `Button`, `Input`, `Badge`, `Dialog`, etc. if one already exists.
- If a matching component exists, use it and import it.
- If it truly doesn't exist, create it once in the right place (`components/ui/`) and use it everywhere — never inline the same styled element twice.
