# Functions: Parametric Values

While you're likely quite familiar with functions already, let's pause for a moment and think about what a function really is, mathematically speaking. This will be helpful when learning about generics.

A function can be though of a map from one value to another. To represent any function, simply write down a list of all inputs and the corresponding output; for example, the logical NOT can be represented as:

[cols="1,1"]
|===
| Input value | Output value
| `true`  | `false`
| `false` | `true`
|===

Since the input is of type `boolean`, there are only two possible inputs (`true` and `false`), and thus the table contains two rows. The type of this function is:

```typescript
// Not valid TypeScript
(boolean) => boolean
```

Unfortunately, https://github.com/microsoft/TypeScript/issues/13152#issuecomment-269099764[TypeScript requires] that the parameter has an arbitrary name–-even though it serves no purpose (other than documentation, possibly):

```typescript
// Correct
type Not = (a: boolean) => boolean
```

Functions with multiple arguments can be thought of functions with a single argument where the argument is a tuple; for example, the logical AND can be represented as:

[cols="1,1"]
|===
| Input value | Output value
| (`true, true`)  | `true`
| (`true, false`) | `false`
| (`false, true`) | `false`
| (`false, false`)| `false`
|===

Where the type of this function is:

```typescript
type And = (a: boolean, b: boolean) => boolean
```

In JavaScript, you _can_  implement functions as maps where the input-output pairs are key-value pairs. While the example above with the logical operations would be contrived, consider a more realistic scenario that maps color names to their RGB values:

```typescript
const colors = {
  red: [255, 0, 0],
  green: [0, 255, 0],
  blue: [0, 0, 255],
}
```

Now you can "call" this function by indexing:

```javascript
console.log(colors['red'])
```

To represent a function that takes a number as argument this way, you would need an object with 2^64^ properties, so instead, functions are normally represented as function expressions:

```javascript
const isPositive = (value) => value > 0
```

In TypeScript, you can annotate the identifier of a function like any value:

```typescript
type IsPositive = (value: number) => boolean
const isPositive: IsPositive = (value) => value > 0
```

Alternatively, annotate the parameters and the return type directly:

```typescript
const isPositive = (value: number): boolean => value > 0
```

A function can thus be thought of in two different ways:

1. As a value (object) that lists all input-output pairs.
2. As an expression that you need to hand a value (as input) before you get a value (as output) back.
