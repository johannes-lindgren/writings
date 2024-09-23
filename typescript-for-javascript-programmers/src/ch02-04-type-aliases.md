# Type Aliases (`type`)

A type expression is an expression that evaluates to a type, such as:

```typescript
true | false
```

In TypeScript, you can alias such expressions with _type aliases_:

```typescript
type TrueOrFalse = false | true
```

`TrueOrFalse` becomes a type that contains the values `false` and `true`, and can be used as any other type:

```typescript
const amIHappy: TrueOrFalse = true
```

Since `TrueOrFalse` contains the exact same number of values as `boolean`, these two types are equivalent to each other--they're just different names for the same type. You can view the `boolean` type as being a type alias for `true | false`:

```typescript
// Pseudo code
type boolean = false | true
```

NOTE: Type aliases are always written in _PascalCase_.
