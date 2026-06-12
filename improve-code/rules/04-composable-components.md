# Rule 4 — Make components composable (shadcn/ui style)

**Trigger:** a component that accepts content via props (`title="..."`, `header={<...>}`, `footer={<...>}`) instead of letting callers compose the structure — i.e., the caller cannot control what goes inside each slot without passing a prop.

**Action:** split the component into named sub-components that callers assemble themselves.

## Example

```tsx
// Before — caller has no control over structure
<Card title="Welcome" content={<p>Hello</p>} />

// After — caller composes
<Card>
  <CardHeader>Welcome</CardHeader>
  <CardContent><p>Hello</p></CardContent>
</Card>
```

## Sub-component rules

Each sub-component (`CardHeader`, `CardContent`, `CardFooter`, …) must:
- Accept `children` and `className`
- Forward a `ref` if it wraps a DOM element
- Merge classes with `cn()` so callers can override styles

```tsx
const CardHeader = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div ref={ref} className={cn('p-6 font-semibold', className)} {...props} />
))
CardHeader.displayName = 'CardHeader'
```

Export all sub-components from the same file. The parent (`Card`) owns only the outer shell — no logic about what its children contain.

## Also apply

- Replace any `isCompact` / `isAdmin` boolean that switches between two completely different layouts with a `variant` prop and a lookup object.
- Ensure every component accepts and forwards `className` so callers can style without CSS overrides.
