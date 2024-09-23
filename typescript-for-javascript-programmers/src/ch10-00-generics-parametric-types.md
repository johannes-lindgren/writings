# Generics: Parametric Types


Similarly to the relationships between values and functions, a type can be parameterized with a _type parameter_. That is, to construct the type, we first need to provide a type for the parameter.

For example, consider a table that maps one set of type to another type, and let's figure out what it means:

[cols="1,1"]
|===
| Input type | Output type
| `number`  | `[number, number]`
| `string`  | `[string, string]`
| `boolean` | `[boolean, boolean]`
| ...   | ...
|===
.What would be a suitable name for this parameterized type? The answer is in the text below.

Please note that the entries in the table are not values, they are _types_. What we are dealing with is a __kind of__ function that you give a type and returns a new type back to you--a parameterized type, or in TypeScript more commonly referred to as _generic_ type.

NOTE: The word choice _generic_ is unfortunate--it's a relic from Java, which has generic classes. A more suitable name would be _parametric type_.

If the syntax for parameterized types and types was consistent with the syntax for values and functions, we _would_ write it as such:

```typescript
// Pseudocode
<T> => [T, T]
```

Instead, we write:

```typescript
type Pair<T> = [T, T]
```

That's right: the table above denotes a pair! `Pair` is a _sort of_ function that accepts one type as an argument and returns a new type that is constructed from the type argument. Since `Pair` in itself is not a type, we cannot annotate identifiers with it without providing a type argument:

```typescript
// Incorrect: `Pair` is not a type
const pair: Pair = [1, 2]
```

If we want to annotate a value with this generic, we first need to construct a type from it by passing a type as an argument:

```typescript
const couple: Pair<string> = ['Sissi', 'Franz Joseph']
```

This is equivalent to:

```typescript
const couple: [string, string] = ['Sissi', 'Franz Joseph']
```

Type parameters are types as any other, and we can arbitrarily construct new types with it.

```typescript
type HttpOkResult<T> = {
  statusCode: 200,
  body: T
}
// `T` gets substituted with the type `{ content: unknown }`
const storyResult: HttpOkResult<{ content: unknown }> = {
  statusCode: 200,
  body: {
    content: {
      title: 'hello',
      text: 'Hello my friend...',
    }
  }
}
```
