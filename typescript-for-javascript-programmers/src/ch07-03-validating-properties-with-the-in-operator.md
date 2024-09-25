# Validating Properties With the in Operator

If the `typeof` operator returns `object`, you also need to check that the value is not `null`:

```typescript
const obj: unknown = ...
if(typeof obj === 'object' && obj !== null) {
  console.log('The type is `object`')
}
```

If the type of a value is `object`, you can use the `in` operator to check whether a property on that object exists:

```typescript
const val: unknown = ...
if(typeof val === 'object' && val !== null && 'id' in val) {
  console.log('The type is `{ id: unknown}`')
}
```

Finally, given all of these checks, you can safely check the type of the property:

```typescript
const val: unknown = ...
if(typeof val === 'object' && val !== null && 'id' in val && typeof val.id === 'number') {
  console.log('The type is `{ id: number }`')
}
```
