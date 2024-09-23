# Optional Properties in Mapped Types

We saw how we can define a parameterized type that makes all properties into unions with `undefined`. But a property being `undefined` is not the same as being absent. For example, this is perfectly valid:

```typescript
// Ok: all properties are present
const user: Undefinable<User> = {
  id: 1,
  name: 'August',
  email: undefined,
}
```

but is not, for one of the properties is missing:

```typescript
// Error: `name` is missing
const user: Undefinable<User> = {
  id: 1,
  name: 'August',
}
```

To make properties _optional_ in mapped types, we can use the `?:` notation:

```typescript
type Partial<T> = {
  [Key in keyof T]?: T[Key]
}
```

This generic type takes any object type `T` and returns a new object type where all properties are optional.

Now we can write our code as such:

```typescript
// The email is missing, and that's okay!
// We can also assign `undefined` values to all properties
const user: Partial<User> = {
  id: 1,
  name: undefined,
}
```

To do the reverse--to make all properties required--use the `-?:` notation:

```typescript
type Required<T> = {
  [Key in keyof T]-?: T[Key]
}
```

TIP: TypeScript provides several so-called utility types to facilitate common type transformations. `Partial` and `Required` are two of those. You can use the utility types without fully understanding TypeScript generics. See the https://www.typescriptlang.org/docs/handbook/utility-types.html[full reference on typescriptlang.org]--they will come in handy!
