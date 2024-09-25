# Type Assertions with `unknown`

There is one exception to this rule: the `unknown` type. Even though the `unknown` type is the superset of all types, it can be asserted to any subtype. But this is mathematically incoherent, and it opens the door to a trick that lets you circumvent the type system: by asserting a type as `unknown`, you can then assert the unknown type as any other type without a type error:

```typescript
const id = '123' as unknown as number
```

Now, TypeScript will consider `id` as a number, when it in fact is a string! In some niche cases, it can be useful to override the type checker when you are absolutely certain that you know better than TypeScript. But needless to say, once you do this, TypeScript will no longer be able to save you from runtime errors. Use `as` with great caution!