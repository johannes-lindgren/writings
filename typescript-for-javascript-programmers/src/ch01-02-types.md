# Types

In JavaScript we deal exclusively with values:

```javascript
const age = 42
```

A value is something that can be stored in memory while the program is running. A value identifier points to a value. The convention is to name value identifiers with lower camel case.

In TypeScript, we also consider the _sets of values_ that our value identifiers are allowed to reference: these sets are called _types_. We can create identifiers that refer to types, and the convention is to name these with upper camel case. For example, we could construct a type `Digit` that represents the set of the numbers 0–9:

![](images/Digit.svg)

We can now annotate a value `digit` with the type `Digit` to tell TypeScript that whatever value is in `digit`, it must be one of the values in `Digit`:

```typescript
const digit: Digit = 5
```

If you assign a value that is not in the annotated type, TypeScript will generate compile-time error:

.This will yield a type error, because `10` is not in `Digit`--the set of numbers 0–9.
```typescript
const digit: Digit = 10
```

Note that you can still run the program. This is because when TypeScript code is compiled, all type annotations are removed. This is what the compiled output looks like:

```javascript
const digit = 10
```

TypeScript is thus a tool that lets us put constraints on the values that our value identifiers can point to. The general idea is that the more constraints we set up, the easier it will be to reason about our program before it runs.
