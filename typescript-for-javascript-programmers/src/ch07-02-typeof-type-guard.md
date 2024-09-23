# The typeof type guard

If you have a union between other types, for example, `string | number`, or `unknown`, use the `typeof` operator to check the type at runtime:

```typescript
const value: unknown = ...
if(typeof value === 'number') {
  console.log('Double the value', value * 2)
}
```

If `typeof value === 'number'` is true, TypeScript infers that the type of `value` is `number` _inside the conditional block_. This allows the use of `value` in the arithmetical expression.

This is how TypeScript infers the type based on the string in the `typeof === ` expression:

- `typeof x === 'undefined'` infers the type of `x` as `undefined`
- `typeof x === 'null'` infers the type of `x` as `object`.
- `typeof x === 'number'` infers the type of `x` as `number`
- `typeof x === 'string'` infers the type of `x` as `string`
- `typeof x === 'boolean'` infers the type of `x` as `boolean`
- `typeof x === 'symbol'` infers the type of `x` as `symbol`
- `typeof x === 'bigint'` infers the type of `x` as `bigint`

NOTE: `typeof x === 'object'` infers the type of `x` as `object | null` because `typeof null === 'object'` is true. This is due to a https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof#typeof_null[historical mistake] in the JavaScript language design, and is not something that TypeScript can fix.

Non-primitive values are inferred as:

- `typeof x === 'function'` infers the type of `x` as `function`
- `typeof x === 'object'` infers the type of `x` as `object | null`
