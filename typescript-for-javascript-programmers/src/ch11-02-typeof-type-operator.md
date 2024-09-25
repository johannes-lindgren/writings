# The `typeof` type operator

Consider a value like the one below:

```typescript
function fetch(input: RequestInfo | URL, init?: RequestInit): Promise<Response> {
  // ...
}
```

Imagine that you want to implement a wrapper with the same API; you'd need to annotate your function with the same types:

```typescript
type Fetch = (input: RequestInfo | URL, init?: RequestInit) => Promise<Response>
const myFetch: Fetch = (input, init) => {
  // ...
}
```

However, TypeScript had already inferred this type from the `fetch` function--it just didn't provide a type alias that you could refer to. The `typeof` type operator lets you extract the type of a value:

```typescript
const myFetch: typeof fetch = (input, init) => {
  // ...
}
```

Note that this is not the same operator as the `typeof` _runtime_ operator, which gets executed and returns a string value. But the `typeof` _type operator_ is evaluated at compile time and can thus only be used in type expressions.
