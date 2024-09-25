# number

You could create a `Digit` type that contains the numbers 0–9:

```typescript
type Digit = 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

Then _imagine_ that you could extend this to all JavaScript numbers:

```typescript
// Pseudo code
type NaturalNumbers = 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | ...
type Integer = ... | -10 | -9 | -8 | -7 | -6 | -5 | -4 | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | ...
type FloatingPointNumbers = ... | 0 | ... | 0.0000000000001 | ... | 0.0000000000002 | ...
```

Then you could think of the `number` type as being defined as the type that contains all integers, floating point numbers, `Infinity`, `-Infinity`, and `NaN`.

```typescript
// Pseudo code
type number = Integer | FloatingPointNumbers | Infinity | -Infinity | NaN
```

This is the `number` type.
