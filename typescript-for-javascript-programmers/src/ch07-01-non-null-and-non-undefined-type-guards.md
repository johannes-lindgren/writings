# Non-null and Non-undefined Type Guards

A nullable or optional value has a type that is a union with `null` or `undefined`; for example, `string | null` and `number | undefined`. You can perform checks for `null` and `undefined` with the `!==` and `===` operators, respectively:

```typescript
const nullable: string | null = ...
const optional: string | undefined = ...
if(nullable !== null) {
  console.log('Not null', nullable)
}
if(optional !== null) {
  console.log('Defined', optional)
}
```