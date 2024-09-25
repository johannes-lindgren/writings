# Discriminated/tagged unions & Pattern Matching


Object types, combined with unions lets us define discriminated unions (aka tagged unions).

For example, consider the case when we want to represent the outcome of a calculation:

1. Success
2. Failure

We _could_ represent this with a single structure with optional properties.

```typescript
type Result = {
  data?: string
  error?: Error
}
```

And represent a result like this

```typescript
const ok: Result = {
  data: 'Hello!'
}
const error: Result = {
  error: new Error('arg!')
}
```

But what would the following data represent?

```typescript
const what: Result = {
  data: 'success!',
  error: Error('... and also failure?!')
}
const ehmm: Result = {}
```

With discriminated unions, we can define an API that _only can represent valid states_:

```typescript
type Success = {
  tag: 'success'
  data: string
}

type Failure = {
  tag: 'failure'
  error: Error
}

type Result = Ok | Err

// Correct
const ok: Result = {
  tag: "success",
  data: 'Hello!'
}
const fail: Result = {
  tag: 'failure',
  error: new Error('Crash! Boom! Bang!')
}
```

As you can see, the `tag` property determines whether the `data` or the `error` properties are defined; there is no way both of these properties can be present or absent at the same time.

By using a switch statement on the `tag` property, TypeScript is able to infer the types of the other properties in the `case` blocks:

```typescript
const res = ok as Result
switch (res.tag) {
  case "success":
    console.log('We won: ', res.data)
    break
  case "failure":
    console.log('We disappointed...', res.error)
}
```

This is called _pattern matching_.
