# Generic Mapped Types

In the previous example, `Key` is a type parameter—analogous to the argument in the callback function of `Array.prototype.map()`—and can be used on the right side of the colon to construct the object type:

```typescript
// Result: { a: 'a', b: 'b' }
type A = {
  [Key in 'a' | 'b']: Key
}
```

This is a silly example, but when used in parameterized (generic) types, `Key` can be used to _index the type parameter_.

```typescript
type Nullable<T> = {
  [Key in keyof T]: T[Key] | null
}
```

This parameterized type takes any object type `T`, maps each key `K` in `T` to the corresponding property value `T[K]`, and forms a union with `null`. The type that is returned is thus a variant of `T` where all properties are nullable; for example:

```typescript
type User = {
  id: number
  name: string
  email: string
}
// Result: { id: number | null, name: string | null, email: string | null }
type NullableUser = Nullable<User>
```

We can do the same with `undefined`:

```typescript
type Undefinable<T> = {
  [Key in keyof T]: T[Key] | undefined
}
```
