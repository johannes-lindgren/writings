# The instanceof Operator

If you use the `instanceof` operator, TypeScript infers the type of the value as the type on the right side of the operator:

```typescript
const value: unknown = ...
if(value instanceof Date) {
  console.log('The type is `Date`')
}
```