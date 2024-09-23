# Mapped Object Types

Forget about types for a moment and consider _values_: how can you construct an object from a set of keys? To be able to tie the solution into TypeScript, the solution should be written in a declarative, functional style. Here's a three-step plan:

1. Define a set of keys (as an array of string)
2. Map each key to a key-value pair (with `Array.prototype.map()]`)
3. Construct an object from all key-value pairs (with `Object.fromEntries()`)

Let's take a concrete example with vectors, where we want to construct an origin (all values zeros) from a set of dimensions:

```typescript
// 1) Define a set of keys
const dimensions = ['x', 'y', 'z']
// Result: { x: 0, y: 0, z: 0 }
const origin =
  // 3) construct an object from all key-value pairs
  Object.fromEntries(
    // 2) map each key to a key-value pair
    dimensions.map((dimension) =>
      [dimension, 0]
    )
  );
```

Now let's translate this question into TypeScript: how can we construct an object type from a set of keys? We'll use the same three-step plan:

1. Define a set of keys--in the type system, this is represented with a union of string literals
2. Map each key to a key-value pair (with a mapped type)
3. Construct an object from all key-value pairs (with an object type)

Let's consider the example with vectors, but this time we construct a type from the set of keys:

```typescript
// 1) Define a set of keys
type Dimension = 'x' | 'y' | 'z'
// Result: { x: number, y: number, z: number }
type CartesianVector3 =
// 3) construct an object from all key-value pairs
{
  // 2) map each key to a key-value pair
  [Key in Dimension]: number
}
```

Although the syntax differs a bit from JavaScript—there's no `map` or `fromEntries`—the principle is the same: the type is constructed by mapping over a set of keys.

Earlier, we saw how we could use the `keyof` operator to derive a type alias for the keys from an object type. We now know how to use mapped types to derive the object type from the keys:

```typescript
type Color = 'primary' | 'secondary'
type Palette = {
  [Key in Color]: {
      main: string
      contrast: string
  }
}
```