# Generic Function Types

Generics can be used to construct any kind of type; for example functions:

```typescript
type Defer<T> = (value: T) => Promise<T>
```

Here `Defer<T>` is a function that wraps an argument in a promise. The argument can be any type, for example:

```typescript
type DeferString = Defer<string>
const deferString: Defer<string> = (payload) => Promise.resolve(payload)
```

But what if we want to have the same function for other types? With `Defer`, we would have to write:

```typescript
const deferBoolean: Defer<boolean> = (payload) => Promise.resolve(payload)
const deferNumber: Defer<number> = (payload) => Promise.resolve(payload)
```

The implementation is the same, so we shouldn't have to define multiple functions. The function body wraps the argument in a _container_, but it does not make any assumption of the content of that container. Therefore, we should be able to parameterize the type of the argument.

Here's another example:

```typescript
type ReverseArray<T> = (items: T[]) => T[]
const reverseNumbers: ReverseArray<number> = (items) => items.reverse()
```

What if we try this:

```typescript
// Incorrect
const reverseNumbers: ReverseArray<T> = (items) => Promise.resolve(items)
```

Unfortunately, this does not work in TypeScript because TypeScript will interpret `T` as a concrete type--not as a type argument. Inconveniently, for generic functions, we need to inline the type argument in the function expression:

```typescript
const reverse = <T>(items: T[]) => items.reverse()
```

which has the intended effect of creating a _generic function_, which we can invoke by also providing type arguments:

```typescript
const reversedAlphabet = reverse<string>(['a', 'b', 'c', 'd', 'e', 'f'])
const reversedDigits = reverse<number>([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

The `reverse` function now both a regular function _and_ a parameterized type.

In most cases when dealing with generic functions, TypeScript will be able to infer the type arguments from the function arguments, so you don't have to provide them explicitly:

```typescript
// `string` is inferred from the function argument
const reversedAlphabet = reverse(['a', 'b', 'c', 'd', 'e', 'f'])
// `number` is inferred from the function argument
const reversedDigits = reverse([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```
