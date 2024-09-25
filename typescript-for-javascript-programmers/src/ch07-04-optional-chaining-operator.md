# The optional chaining operator (`?.`)

If you have a deeply nested object with optional properties, it gets verbose to check for `undefined` values with the equality operator (`===`). Use the _optional chaining operator_ (`?.`) to check whether a property exists:

```typescript
const obj: { prop?: number }
console.log(obj.prop?.toFixed(2))
```

The optional chaining operator is a shorthand for the following:

```typescript
const obj: { prop?: number }
console.log(obj.prop === undefined ? undefined : obj.prop.toFixed(2))
```
