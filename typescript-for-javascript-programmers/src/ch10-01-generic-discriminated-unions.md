# Generic Discriminated Unions

Generics (parametric types) are especially handy when combined with records and unions. With these three constructs, we can model any kind of data.

Let's revisit the tagged unions that we defined earlier where we defined this discriminated union:

```typescript
type Ok = {
  tag: 'success'
  data: string
}

type Err = {
  tag: 'failure'
  error: Error
}

type OkOrFailure = Ok | Err
```

Wouldn't it be great if the `data` property was not bound to a specific type? If it was parameterized with a type parameter, we could re-use the `Result` type for different kinds of data:

```typescript
type Ok<T> = {
  tag: 'ok',
  data: T
}
type Err = {
  tag: 'error',
  error: Error
}
type Result<T> = Ok<T> | Err
```

This can be used as in the example:

```typescript
const okResult: Result<number> = {
  tag: 'ok',
  data: 1123,
}
const errorResult: Result<number> = {
  tag: 'error',
  error: new Error('Kaboom!'),
}
```

If we want, we can parameterize `Result` with two type parameters:

```typescript
type Result<Data, Err> = Ok<Data> | Err<Err>
type OkResult<T> = {
  tag: 'ok',
  data: T
}
type ErrorResult<E> = {
  tag: 'error',
  error: E
}
```

For convenience, we could let the `Error` parameter default to type `Error`

```typescript
type Result<Data, Err> = Ok<Data> | Err<Err>
```
