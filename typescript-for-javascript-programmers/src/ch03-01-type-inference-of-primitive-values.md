# Type Inference of Primitive Values

When you assign a value to a variable, TypeScript infers the type of the variable based on the type of the assigned value. In the example below, `thomas` is of type `User`. When the variable `user` is assigned `thomas`, the type inferred type is also `User`:

.The type of `user` is inferred as `User`
```typescript
const thomas: User = ...
const user = thomas
```

Unfortunately, there is one inconsistency in the type inference mechanism: TypeScript does not infer the type of value literals as the corresponding type literal; in the example below, the variable `pi` is inferred as `number`, not `3.14159`:

.The type of `pi` is inferred as `number`
```typescript
const pi = 3.14159
```

and string literals are inferred as `string`:

.The type of `defaultLogLevel` is inferred as `string`
```typescript
const defaultLogLevel = 'info'
```

Here's how TypeScript infers primitive values:

* numbers (`1`, `0.5`, `NaN`, `100`) are inferred as `number`
* strings (`'hello'`, `"world"`) are inferred as `string`
* booleans (`true`, `false`) are inferred as `boolean`
* `undefined` is inferred as `undefined`
* `null` is inferred as `null`
* `Symbol` is inferred as `symbol`
* `bigint` is inferred as `bigint`

To infer it as the literal type, you can annotate the use a _type assertion_:

.The type of `logLevel` is inferred as `'info'`
```typescript
const logLevel = 'info' as 'info'
```

To make it more convenient, use an `as const` expression:

.The type of `logLevel` is inferred as `'info'`
```typescript
const logLevel = 'info' as const
```
