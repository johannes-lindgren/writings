# User-defined Type Guards (Type Predicates)

We saw that validation of objects generates a lot of boilerplate code. You could extract the code like this

```typescript
type Entity = {
  id: number
}
function isEntity(value: unknown): boolean {
  return typeof value === 'object' && value !== null && 'id' in value && typeof value.id === 'number'
}
```

However, if you use this in an if-statement, TypeScript can no longer infer the type of the value:

```typescript
const value: unknown = ...
if(isEntity(value)) {
  console.log('Id', value.id) // <-- Type Error, since 'id` doesn't exist on `unknown`
}
```

The reason is that the type signature of `isEntity` reveals nothing about the type guard. You can include a _user-defined type guard_ to fix this:

```typescript
type Entity = {
  id: number
}
function isEntity(value: unknown): value is Entity {
  return typeof value === 'object' && value !== null && 'id' in value && typeof value.id === 'number'
}
```

This function still returns a boolean, but if the return value is `true`, TypeScript infers that the type of `value` is `Entity`. The expression `value is Entity` is called a _type predicate_.

CAUTION: The type predicate does not need to match the inferred type in the function body: TypeScript will simply trust that the predicate is accurate. In the example above, we could have written `value is null`, and TypeScript wouldn't have generated an error. So whenever you create a user-defined type guard, include unit tests to ensure that the type guard is accurate.

