# Ignoring Errors

A strongly typed language like TypeScript has the capability to analyze a program and prove whether it is correct, but it cannot do the opposite--that is, to prove whether a program is _incorrect_.

NOTE: A type error only indicates that the compiler cannot guarantee the program's correctness——it can still be functioning correctly even with type errors.

However good the TypeScript compiler is to reason about your code, there will arise scenarios where the programmer knows better than the type checker and thus want to override the type checker's decision. In these cases, you can use the `@ts-ignore` directive to tell TypeScript to ignore the type error:

.A function that takes a list of strings and returns a record that maps the index of the string to the string itself.

```typescript
export const calculateZIndices = <const Keys extends string[]>(
    keys: Keys,
): { [key in Keys[number]]: number } =>
    // @ts-ignore
    Object.fromEntries(keys.map((key, index) => [key, index]))
```

This avoids the following error:

```text 
TS2322: Type { [k: string]: number; } is not assignable to type { [key in Keys[number]]: number;
```

Which, if you look closely at the code, is actually an inaccurate error message.

However, this feature should be used with great caution. It not only forces you to outperform TypeScript in your analysis of the program, but it can severely compromise the maintainability of the code.

CAUTION: A good rule of thumb is to never use `@ts-ignore`.

TIP: Whenever you _do_ use `@ts-ignore``, ensure that you test the code thoroughly with automated tests.
