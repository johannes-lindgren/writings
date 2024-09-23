# Arrays

Combining With tuples and union types, we can create arrays of limited length:

```typescript
type UpToTwoNumbers = [] | [number] | [number, number]
```

This array can have 0, 1, or 2 elements. This is not a common use case, but consider instead what happens when we expand the series to infinity:

```typescript
// Pseudo code
type number[] = []
  | [number]
  | [number, number]
  | [number, number, number]
  | [number, number, number, number]
  | ...
```

This gives us an array of any length. While the above example is just pseudo code, some languages do in fact define arrays like this.

We can create arrays of different types:

```typescript
const numbers: number[] = [1, 2, 3, 4, 5, 6, 7, 8]
const booleans: boolean[] = [false, true, false]
```
