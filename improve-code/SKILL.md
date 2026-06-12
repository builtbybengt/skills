---
name: improve-code
description: Review and improve React/frontend code quality. Use when asked to improve code, refactor components, clean up state, extract hooks, or make components more reusable and composable.
---

Audit the codebase (or specified files) and apply the rules below in order. Each rule has a clear trigger and a concrete action — do not refactor speculatively.

## Rules (apply in this order)

1. [Eliminate useless useState](rules/01-derive-state.md)
2. [Extract components](rules/02-extract-components.md)
3. [Extract custom hooks](rules/03-extract-hooks.md)
4. [Make components composable](rules/04-composable-components.md)
5. [Use existing components — never reinvent](rules/05-use-existing-components.md)

## Process

1. **Read first.** Scan the files named in the request (or the full `src/` tree for broad requests). Also scan `components/`, `ui/`, and `shared/` to build a mental map of what already exists. Do not edit yet.
2. **List findings.** For each rule triggered, note: file, line range, what to change, why.
3. **Apply in rule order.** Each change is small and focused.
4. **Do not chain.** Fix what the rules call for. Do not rename variables, reformat, or add features while you're in there.

## What NOT to do

- Do not extract a component just because a function returns JSX — only extract when there's a real boundary.
- Do not add `useMemo`/`useCallback` without a measured reason — premature memoization is its own form of clutter.
- Do not rewrite working logic while refactoring structure.
- Do not introduce new abstractions (base classes, HOCs, render props) unless the rules above specifically call for it.
