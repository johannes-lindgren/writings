# TODO: Coming Soon (hopefully)

These are the TypeScript-related topics that haven't yet been covered in the book, but which I intend to include in the future:

- `extends` type constraint
- `extends` conditional types, and `infer`
- covariance and contravariance
- type declarations
  -- module augmentation
- `{}` type
- `object` type
- Compiler options and `@typescript-eslint`
- nominal vs structural type system.
- Exact and inexact object types (extra properties allowed)
  - `satisfies`
- Testing generics
  - `@ts-expect-error` for testing generic types
- Index Signatures (`[key: string]: T`)
- Patterns to avoid: a chapter that covers some of the worst features of TypeScript—just so that you can avoid them.
  - `any`
  - `enums`
  - `interface`
  - Assertion guards (`asserts x is T`)
  - decorators `@foo`
- Out of scope: a chapter that lists some of the features that are deemed out of scope for the book. They are good to know, but not essential for programming with TypeScript. If you encounter them, the official TypeScript documentation will be your friend. 
  - template literals
  - classes
  - `this`
  - `readonly`
- Common pitfalls
  - Misuse of constants (use type literals)
