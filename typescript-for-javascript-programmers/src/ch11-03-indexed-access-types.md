# Indexed Access Types ([])

You can index object types with the `[]` type operator to get the type of a property:

```typescript
type User = {
  id: number
  name: string
  email: string
}
type UserId = User['id'] // number
```

Just like with the `typeof` type operator, the `[]` type operator is evaluated at compile time and can only be used in type expressions. This means that the `'id'` type argument is not a string value, but a type.

This operator can be useful when you want to indicate that another property or function parameter has a relation to the property of an object type:

```typescript
type User = {
  id: number
  parentId: User['id']
}
const getUser = (id: User['id']) => {
  // ...
}
```

If it were not for the `[]` operator, you would only see `parentId: number` in the `User` type alias, and `id: number` in the function signature. Not that if you now change the type of `id` in `User`, the type of `parentId` and `id` in `getUser` will automatically be updated.

Indexed access types can be used with discriminated unions to extract the type that describes all possible tags in the union:

```typescript
type Asset = {
  type: 'image'
  url: string
} | {
  type: 'video'
  url: string
  duration: number
} | {
  type: 'audio'
  url: string
  duration: number
}
type AssetType = Asset['type'] // 'image' | 'video' | 'audio'
```

This is much more convenient than having to define the types twice.

It can also be used to index arrays:

```typescript
// as const so that we infer the tuple `['info', 'warn', 'error']` and not `string[]`
const allLogLevels = ['info', 'warn', 'error'] as const
// `typeof allColors` yields the type `['info', 'warn', 'error']`
type Color = (typeof allColors)[number] // 'info' | 'warn' | 'error'
```

Now all log levels are defined in one place, and you can easily add or remove log levels without having to update the type alias. There's no repetition of the words `'info'`, `'warn'`, and `'error'`.
