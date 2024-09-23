# string

The `string` type contains all strings, and you can think of it in similar terms as the `number` type:

```typescript
// Pseudo code
type string = 'a' | 'b' | 'c' | ... | 'z' | 'aa' | 'ab' | 'ac' | ... | 'az' | 'ba' | 'bb' | 'bc' | ... | 'zz' | 'aaa' | 'aab' | ...
```

Again, this is just pseudo code. In reality, the `number` and `string` types are built-in types in TypeScript, and you cannot redefine them.

### `null` and `undefined`

The `null` and `undefined` types are the types that contain the values `null` and `undefined`, respectively:

```typescript
const nothing: null = null
const notDefined: undefined = undefined
```

As with any literal type, they are most useful when combined with other types:

```typescript
type MaybeNumber = number | undefined
const maybeNumber: MaybeNumber = 42
const maybeNot: MaybeNumber = undefined
```

TIP: whenever you have a choice, prefer to use `undefined` over `null`. `undefined` is a more consistently used in Node.js and DOM APIs, is the result when indexing out of bounds, and is the default value for uninitialized variables.
