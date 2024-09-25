# The unknown Type

Recall how `never` is the type that doesn't contain any values. The `unknown` type is the opposite: it is the type that contains _all_ values:

```typescript
// Pseudo code: `unknown` is built-in to TypeScript
type unknown = boolean | number | string | null | undefined...
```

If an identifier is typed with `unknown`, TypeScript can't infer any information from it, because it can be assigned any value:

```typescript
const a: unknown = 123
const b: unknown = { a: 'a' }
const c: unknown = () => 123
```

You may encounter the `any` type at some point. `any` is the same type as `unknown`, but it also _disables the type checker_. Never ever use it. If you really want to work around the type system, it's better to be explicit.

WARNING: The `any` type disables the type checker—never use it!