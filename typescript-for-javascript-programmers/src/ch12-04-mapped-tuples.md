# Mapped Tuples

You can also map over tuples. Though we normally denote tuples and arrays with `[]`, TypeScript treats them as objects where the properties are numeric:

. Two equivalent ways to denote a tuple type
```typescript
type TripletA = [number, string, boolean]
type TripletB = {
  [0]: number
  [1]: string
  [2]: boolean
}
```

This means that you can create mapped types for tuples with the same syntax as for objects:

```typescript
type NullableTriplet<T> = {
  [Key in keyof T]: (value: T[Key]) => T[Key]
}
```

This utility type takes a tuple type `T` and returns a new tuple type where each element are transformations of the original elements.
