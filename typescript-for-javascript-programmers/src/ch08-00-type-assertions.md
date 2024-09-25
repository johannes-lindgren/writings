# Type Assertions

You will encounter scenarios where you want to initialize a value to `undefined`, but later reassign it to a different value:

```typescript
let user: undefined | User = undefined

// Later...
user = await fetchUser() // Returns a `User`
```

In this case, you must annotate `user` with a type `undefined | User`.

However, in some scenarios where you deal with records, you may have situation where you'd rather use the type inference to its greatest extent; for example, consider a state-like object:

```typescript
const state = {
  user: undefined,
  count: number
}
```

If most properties in the object can be inferred, it would be unnecessarily verbose to annotate it as such:

```typescript

const state: {
  user: User | undefined
  count: number
} = {
  user: undefined,
  count: number
}
```

To save yourself from excessive boilerplate, you can annotate the `user` property with the assertion operator (`as`):

```typescript
const state = {
  user: undefined as undefined | User,
  count: number
}
```

This tells TypeScript to infer `user` as `undefined | User`, instead of just `undefined`. You can also use it as an alternative to the type annotation separator (`:`):

```typescript
let user: undefined | User = undefined
// is equivalent to:
let user = undefined as undefined | User
```

NOTE: that nothing happens with the value on the left side--neither at runtime nor during compile time. When a TypeScript file is compiled into JavaScript, the type annotations are stripped, and you get simply:

You can only use type assertion (`as`) if the value on the left side is a subset of the type on the right side. The following are valid:

```typescript
// Correct ✅
const a = 1 as 1 | 2
const b = 100 as undefined | number
const c = undefined as undefined | number
```

But the following are incorrect:

```typescript
// Incorrect ❌
const a = 1 as 2 | 3
const b = 100 as undefined | string
const c = null as undefined | number
```
