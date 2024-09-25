# Type Inference

So far in our code examples, we have annotated every single identifier with a type:

```typescript
const age: number = 21
const ageAsString: string = age.toString()
```

But if you think for a second about this code, you can easily deduct that the program is correct:

1. `age` is assigned the value `21`, thus `age` must be of type `number`.
2. Since `age` is a number, you can call age.toString()`, which evaluates to a value of type `string`.
3. Therefore, `ageAsString` must be of type `string`

TypeScript is able to perform the same line of reasoning, which means that you can omit the type annotations without getting any type errors:

```typescript
const age = 21
const ageAsString = age.toString()
```

This looks just like JavaScript, and is in fact also a valid TypeScript program. This ability of TypeScript to deduct the type of variables is called _type inferrence_.

1. On the first line, TypeScript infers that the value of `age` is `number`.
2. On the second line, TypeScript infers that the type of `age.toString()` is `string`.
3. Lastly, TypeScript infers that the type of `ageAsString` is `string`.

Why then do we need type annotations? The answer is that when the type cannot be inferred by its usage. For example, in the following code, TypeScript cannot infer the type of `value`:

```typescript
const twice = (value: number) => 2 * value
```

The first argument in the `twice` function is annotated with the type `number`, so that TypeScript can guarantee that whatever goes into the multiplication is a number. More on <<_functions, functions later>>.
