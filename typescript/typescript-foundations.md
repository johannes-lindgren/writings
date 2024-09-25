# TypeScript Foundations for JavaScript Programmers
Johannes Lindgren
:imagesdir: images
:toc:
:stem:

This is a brief introduction to TypeScript for programmers who are already familiar with JavaScript. We'll start with the most basic language features and work ourselves up to more advanced features that lets us utilize the full power of the type checker. We'll cover topics such as:

- type annotation, type checking, type inference, type guards, type predicates, and type assertions.
- literal types and primitive types
- union types (sum types)
- tuples, and records (product types)
- arrays (recursive types)
- function types (exponential types)
- generic types (parametric types)

However, before delving deep into TypeScript's syntax, let's first explore what motivates us to adopt TypeScript in the first place, and how TypeScript relates to JavaScript.

## Why TypeScript?

The main reason to adopt TypeScript is to establish guarantees of how JavaScript runs. One concrete example is to prevent code from crashing; it is preferable to give the programmer an error in the IDE than to let the client experience an incident in production. Unfortunately, it is not so easy as to simply enable TypeScript in an existing project: before you may reap the benefits, you must first start programming in a much more restrictive way. Going from untyped languages to types languages is a shift in the programming paradigm. Since the advent of computing, programming languages have been following a trend of becoming more restrictive while giving more compile-time guarantees:

- Programming started with raw machine code and Assembly languages that directly translates to machine code. Assembly languages are notoriously difficult to reason about. Therefore, more restrictive languages soon emerged.
- Procedural languages, such as Fortran, contains the programmer to deal with variables, rather than registers and memory addresses.
- Structured languages such as C favors conditional blocks, loops, and subroutines over `goto` statements; which creates a more reasonable control flow.
- Typed languages such as C++ prevents the programmer from using memory in the wrong way; for example by indexing an arrat with a floating point number.
- Garbage-collected languages prohibits you from allocating and freeing up memory directly, but helps to prevent memory leaks.
- Pure functional languages prohibits you from mutating values, which guarantees that the program can be run concurrently.

With every new paradigm on this list, the programmer is more and more limited in what code they are able to produce; but in return, they are better able to write bug-free code. TypeScript is no different in that it adds new constraints to JavaScript; it prohibits you from using data in _potentially_ incorrect ways, which guarantees that you don't run into runtime exceptions. TypeScript limits what code you'd otherwise be allowed to write in JavaScript, but in turn gives you certain guarantees of how your program behaves at runtime. You're trading freedom against:

- Fewer runtime errors—by analyzing the structure of your data and functions
- Improved refactoring—IDEs can better understand how your code is connected
- Improved documentation—from the type signature

### What is TypeScript—a Language or a Linter?

TypeScript started out as a compiled programming language similar to JavaScript, but with types, classes, and other exotic features that were left out from an aging JavaScript (ECMAScript) standard. But as new standards were released and implemented by the browsers, most of the features that TypeScript initially brought to the ecosystem were added to the JavaScript language itself. TypeScript today is no longer widely used to compile JavaScript code—this task is left to the bundlers—but its sole purpose is to serve as a _linter_ which analyzes the code for imperfections before the compiled code ever runs.

When you write TypeScript code such as this:

```typescript
const pi = 3.14159
console.log('tau', 2 * pi)
```

TypeScript verifies that the operations on all values can be performed without runtime errors. The example above will pass the type checker, because TypeScript recognizes that both `pi` and `2` are numbers, which can be multiplied.

If you instead would have written:

```typescript
const pi = '3.14159'
console.log('tau', 2 * pi)
```

TypeScript would have generated a _type error_ in the IDE, because multiplying a string with a number would have generateda runtime error.

Note that the code above does not contain any TypeScript-specific syntax, yet TypeScript was able to analyze and catch the error regardless. TypeScript also bring its own syntax to the game, such as type annotations:

```typescript
const pi: number = 3.14159
console.log('tau', 2 * pi)
```

This code cannot run in the browser, because the type annotations are not valid JavaScript. When you compile TypeScript code into JavaScript code, the types are simply eliminated from the output. The code above would be compiled into:

```typescript
const pi = 3.14159
console.log('tau', 2 * pi)
```

The type checking is a separate process from the compilation, hence why TypeScript nowadays is regularly used as a linter, but seldom as a compiler.

In this sense, we can understand TypeScript more as a powerful linter, rather than an entirely different programming language.

Excluding the type annotations (and a couple of TypeScript-specific features), all valid TypeScript programs are valid JavaScript programs. But not all valid JavaScript programs are able to pass TypeScript's type checker. While you might have heard otherwise, in this sense, TypeScript is a _subset_ of JavaScriptWith—not the other way around:

.All programs that pass the type checker are valid JavaScript programs, but not all valid JavaScript programs pass the type checker; hence TypeScript is a subset of JavaScript.
image::ts-js-subset.svg[]

NOTE: Because TypeScript adds new syntax and features to the language, from a certain point of view, TypeScript can be considered a superset of JavaScript: while most JavaScript programs cannot pass the type checker, all be compiled by TypeScript; but not all TypeScript programs can be run as JavaScript. Though, since TypeScript is seldom used as a compiler nowadays, this point of view is less relevant.

## Types

In JavaScript we deal exclusively with values:

```javascript
```
const age = 42
```

A value is something that can be stored in memory while the program is running. A value identifier points to a value. The convention is to name value identifiers with lower camel case.

In TypeScript, we also consider the _sets of values_ that our value identifiers are allowed to reference: these sets are called _types_. We can create identifiers that refer to types, and the convention is to name these with upper camel case. For example, we could construct a type `Digit` that represents the set of the numbers 0–9:

image:Digit.svg[]

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

## Primitive Types

We're going to explore the various types in TypeScript, starting with the most primitive types, and then moving on to more complex, composite types.

### The `never` type

Since a type represents a set of values, there exists a type that represents the _empty set_: this type is called `never`.

.The never type doesn't contain any values.
image:never.svg[]

In JavaScript, a value identifier always has a value, even if that value is `undefined`. Therefore, it is impossible to have an identifier with the type `never`. What use it is this type then? Later, we will learn that you can perform various operations on types, and sometimes, the `never` type shows up as a result of these operations. For now, it will not be important, but it's good to know that it exists to understand that types really are like sets.

### Literal Types

If `never` is the simplest type because it doesn't contain any values, the second-simplest category of types is the types that contain a single value: these types are called _type literals_:

```typescript
const nothing: undefined = undefined
```

This just tells us that `nothing` can only ever have one value: `undefined`. Note that the occurrence of `undefined` between the `:` and `=` symbols is actually a type and not a value. For each literal value, there exists a corresponding type with the same name.

> For each literal value, there exists a corresponding type with the same name.

So the values `undefined`, `true`, `false`, `123`, and `"hello"` can be either values or types depending on where in the syntax tree they appear. For example, if a literal appears directly after an assigment operator (`=`), it is a value; but if it appears after the colons (`:`) after a variable declaration, it is a type.

.Literal types contain a single value.
image:primitive-types.svg[]

### Union Type Operator

Value types are not very interesting on their own--they get much more interesting when they're combined into larger types. Consider the two types `true` and `false`:

image:true-and-false.svg[]

TypeScript has _type operators_ that let you combine types in various ways. One of these operators is the _type union operator_ `|`, which lets you combine two types into a new type that contains all values from both operands. Since types correspond to sets, the union operator `|` corresponds to the set union operator stem:[\uu]:

image:boolean.svg[]

In TypeScript, this can be written as such:

```typescript
const amIHappy: true | false = true
```

The expression `true | false` can be read as "true or false". Since it's a type operator, it only evaluated at compile-time by the type checker.

`true | false` is such a common occurrence that TypeScript has a built-in type for it; called `boolean`:

```typescript
const amIHappy: boolean = true
```

NOTE: `boolean` is a primitive type. All primitive types are always written in lowercase.

### Type Aliases (`type`)

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

### `number`

You could create a `Digit` type that contains the numbers 0–9:

```typescript
type Digit = 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

Then _imagine_ that you could extend this to all JavaScript numbers:

```typescript
// Pseudo code
type NaturalNumbers = 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | ...
type Integer = ... | -10 | -9 | -8 | -7 | -6 | -5 | -4 | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | ...
type FloatingPointNumbers = ... | 0 | ... | 0.0000000000001 | ... | 0.0000000000002 | ...
```

Then you could think of the `number` type as being defined as the type that contains all integers, floating point numbers, `Infinity`, `-Infinity`, and `NaN`.

```typescript
// Pseudo code
type number = Integer | FloatingPointNumbers | Infinity | -Infinity | NaN
```

This is the `number` type.

### `string`

The `string` type contains all strings, and you can think of it in similar terms as the `number` type:

```typescript
// Pseudo code
type string = 'a' | 'b' | 'c' | ... | 'z' | 'aa' | 'ab' | 'ac' | ... | 'az' | 'ba' | 'bb' | 'bc' | ... | 'zz' | 'aaa' | 'aab' | ...
```

Again, this is just pseudo code. In reality, the `number` and `string` types are built-in types in TypeScript, and you cannot redefine them.

### `null` and `undefined`

The `null` and `undefined` types are the types that contain the values `null` and `undefined`, respectively:

```typescript
const nothing: null = null
const notDefined: undefined = undefined
```

As with any literal type, they are most useful when combined with other types:

```typescript
type MaybeNumber = number | undefined
const maybeNumber: MaybeNumber = 42
const maybeNot: MaybeNumber = undefined
```

TIP: whenever you have a choice, prefer to use `undefined` over `null`. `undefined` is a more consistently used in Node.js and DOM APIs, is the result when indexing out of bounds, and is the default value for uninitialized variables.

### `symbol` and `bigint`

Finally, you have the primitive data types `bigint` that is the type that contains all https://developer.mozilla.org/en-US/docs/Glossary/BigInt[BigInts], and `symbol` that contains all https://developer.mozilla.org/en-US/docs/Glossary/Symbol[Symbols].

## Type Inference

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

### Type Inference of Primitive Values

When you assign a value to a variable, TypeScript infers the type of the variable based on the type of the assigned value. In the example below, `thomas` is of type `User`. When the variable `user` is assigned `thomas`, the type inferred type is also `User`:

.The type of `user` is inferred as `User`
```typescript
const thomas: User = ...
const user = thomas
```

Unfortunately, there is one inconsistency in the type inference mechanism: TypeScript does not infer the type of value literals as the corresponding type literal; in the example below, the variable `pi` is inferred as `number`, not `3.14159`:

.The type of `pi` is inferred as `number`
```typescript
const pi = 3.14159
```

and string literals are inferred as `string`:

.The type of `defaultLogLevel` is inferred as `string`
```typescript
const defaultLogLevel = 'info'
```

Here's how TypeScript infers primitive values:

* numbers (`1`, `0.5`, `NaN`, `100`) are inferred as `number`
* strings (`'hello'`, `"world"`) are inferred as `string`
* booleans (`true`, `false`) are inferred as `boolean`
* `undefined` is inferred as `undefined`
* `null` is inferred as `null`
* `Symbol` is inferred as `symbol`
* `bigint` is inferred as `bigint`

To infer it as the literal type, you can annotate the use a _type assertion_:

.The type of `logLevel` is inferred as `'info'`
```typescript
const logLevel = 'info' as 'info'
```

To make it more convenient, use an `as const` expression:

.The type of `logLevel` is inferred as `'info'`
```typescript
const logLevel = 'info' as const
```

## The `unknown` type

Recall how `never` is the type that doesn't contain any values. The `unknown` type is the opposite: it is the type that contains _all_ values:

```typescript
// Pseudo code: `unknown` is built-in to TypeScript
type unknown = boolean | number | string | null | undefined...
```

If an identifier is typed with `unknown`, TypeScript can't infer any information from it, because it can be assigned any value:

```typescript
const a: unknown = 123
const b: unknown = { a: 'a' }
const c: unknown = () => 123
```

You may encounter the `any` type at some point. `any` is the same type as `unknown`, but it also _disables the type checker_. Never ever use it. If you really want to work around the type system, it's better to be explicit.

WARNING: The `any` type disables the type checker--never use it!

## Tuples

While unions describe types of that are either "this _or_ that", tuples describes types that embed "this _and_ that".

Tuples are arrays of fixed size, and are annotated with square brackets `[]`. The simplest tuple does not contain any data:

```typescript
type Unit = []
const unit = []
```

It gets more interesting as we embed information in the tuple types:

```typescript
type LineCoordinate = [number]
const x = [10]
type PlaneCoordinate = [number, number]
const planeCoordinate = [10, 45]
type SpaceCoordinate = [number, number, number]
const spaceCoordinate = [10, 45, -125]
```

Because TypeScript knows how many elements the tuple contain, we can destructure them:

```typescript
const [x, y, z] = spaceCoordinate
```

Tuples are sometimes useful when we want to return two or three results from a function. Instead of using parameters as out parameters (as done in languages such as Java), or returning object with names properties, return a tuple. In the following example, TypeScript can infer that `Promise.all` returns a promise of `[string, number, Date]`, because the argument was a tuple:

```typescript
const [name, age, startDate] = await Promise.all([
    Promise.resolve('Eamonn'),
    Promise.resolve(21),
    Promise.resolve(new Date(2012, 9, 1)),
]);
```

## Arrays

Combining With tuples and union types, we can create arrays of limited length:

```typescript
type UpToTwoNumbers = [] | [number] | [number, number]
```

This array can have 0, 1, or 2 elements. This is not a common use case, but consider instead what happens when we expand the series to infinity:

```typescript
// Pseudo code
type number[] = []
  | [number]
  | [number, number]
  | [number, number, number]
  | [number, number, number, number]
  | ...
```

This gives us an array of any length. While the above example is just pseudo code, some languages do in fact define arrays like this.

We can create arrays of different types:

```typescript
const numbers: number[] = [1, 2, 3, 4, 5, 6, 7, 8]
const booleans: boolean[] = [false, true, false]
```

## Object Types

Tuples and arrays lets us encode multiple types into a new type. For example, we could encode a person's name and age in a tuple:

```typescript
type Person = [
  // name
  string,
  // age
  number,
]
```

The problem is that as more items are added to the tuple, it gets more difficult to keep track of which index correspond to which property. Consider what happens if we also include the person's height, the birth year in `Person`: Can you easily tell which index contains the height and which contains the birth year?

```typescript
type Person = [
  string,
  number,
  number,
  number,
]
```

A _record_ (also known as _object_) allows us to label each item:

```typescript
type Person = {
  name: string
  age: string
  height: number
  birthYear: number
}
```

which lets us instantiate an object as

```typescript
const person = {
  name: 'Johannes Kepler',
  age: 58,
  height: 1.76,
  birthYear: 1571,
}
```

By aligning these two types side-by-side, you can easily see that these two structures are mathematically identical, because they contain the same amount of information, but the record/object is more ergonomic:

```typescript
type Person = [
  string,
  number,
  number,
  number,
]
type Person = {
  name: string
  age: string
  height: number
  birthYear: number
}
```

In statically typed programming languages such as C++, the property names of records (classes) do not exist at runtime; in memory, the records are stored as arrays.

## Optional Properties

Sometimes, we want to allow properties to be optional:

```typescript
// Optional
type GeoCoordinateImplicit = {
  latitude: number
  longitude: number
  elevation?: number
}
const k2Peak: GeoCoordinateExplicit = {
  latitude: 35.8825,
  longitude: 76.513333,
  elevation: 8611,
}
const mountEverestPeak: GeoCoordinateImplicit = {
  latitude: 27.988056,
  longitude: 86.925278,
}
```

However, when possible, it's best to be explicit by the property as a union with `undefined`:

```typescript
type GeoCoordinateExplicit = {
  latitude: number
  longitude: number
  elevation: number | undefined
}

const k2Peak: GeoCoordinateExplicit = {
  latitude: 35.8825,
  longitude: 76.513333,
  elevation: 8611,
}
const mountEverestPeak: GeoCoordinateImplicit = {
  latitude: 27.988056,
  longitude: 86.925278,
  elevation: undefined
}
```

This forces the API consumer to consciously set the property to `undefined`.

Just note that these are not identical:

```typescript
// A != B
type A = {
  prop?: number
}
type B = {
  prop: number | undefined
}
// correct
const a: A = {}
const a: A = { prop: 1 }
const b: A = { prop: 1}
// incorrect
const b: A = {}
```

## Type Guards

Consider a type that is a union between two smaller types; for example `number | undefined`:

image:type-guard.diagrams.svg[]

If you want to use the value as a number, you first need to check that it' not `undefined` before you can use it. This is called a _type guard_.

```typescript
const value: number | undefined = ...
if(value !== undefined) {
  console.log('Twice', value * 2)
}
```

TypeScript understands that if the conditional statement gets executed, `value` cannot be `undefined`, and can therefore be used as a number: TypeScript has _narrowed down_ the type from `number | undefined` to `number`.

### Non-null and non-undefined type guards

A nullable or optional value has a type that is a union with `null` or `undefined`; for example, `string | null` and `number | undefined`. You can perform checks for `null` and `undefined` with the `!==` and `===` operators, respectively:

```typescript
const nullable: string | null = ...
const optional: string | undefined = ...
if(nullable !== null) {
  console.log('Not null', nullable)
}
if(optional !== null) {
  console.log('Defined', optional)
}
```

### The `typeof` type guard

If you have a union between other types, for example, `string | number`, or `unknown`, use the `typeof` operator to check the type at runtime:

```typescript
const value: unknown = ...
if(typeof value === 'number') {
  console.log('Double the value', value * 2)
}
```

If `typeof value === 'number'` is true, TypeScript infers that the type of `value` is `number` _inside the conditional block_. This allows the use of `value` in the arithmetical expression.

This is how TypeScript infers the type based on the string in the `typeof === ` expression:

- `typeof x === 'undefined'` infers the type of `x` as `undefined`
- `typeof x === 'null'` infers the type of `x` as `object`.
- `typeof x === 'number'` infers the type of `x` as `number`
- `typeof x === 'string'` infers the type of `x` as `string`
- `typeof x === 'boolean'` infers the type of `x` as `boolean`
- `typeof x === 'symbol'` infers the type of `x` as `symbol`
- `typeof x === 'bigint'` infers the type of `x` as `bigint`

NOTE: `typeof x === 'object'` infers the type of `x` as `object | null` because `typeof null === 'object'` is true. This is due to a https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof#typeof_null[historical mistake] in the JavaScript language design, and is not something that TypeScript can fix.

Non-primitive values are inferred as:

- `typeof x === 'function'` infers the type of `x` as `function`
- `typeof x === 'object'` infers the type of `x` as `object | null`

### Validating Objects (`in`)

If the `typeof` operator returns `object`, you also need to check that the value is not `null`:

```typescript
const obj: unknown = ...
if(typeof obj === 'object' && obj !== null) {
  console.log('The type is `object`')
}
```

If the type of a value is `object`, you can use the `in` operator to check whether a property on that object exists:

```typescript
const val: unknown = ...
if(typeof val === 'object' && val !== null && 'id' in val) {
  console.log('The type is `{ id: unknown}`')
}
```

Finally, given all of these checks, you can safely check the type of the property:

```typescript
const val: unknown = ...
if(typeof val === 'object' && val !== null && 'id' in val && typeof val.id === 'number') {
  console.log('The type is `{ id: number }`')
}
```

### The optional chaining operator

If you have a deeply nested object with optional properties, it gets verbose to check for `undefined` values with the equality operator (`===`). Use the _optional chaining operator_ (`?.`) to check whether a property exists:

```typescript
const obj: { prop?: number }
console.log(obj.prop?.toFixed(2))
```

The optional chaining operator is a shorthand for the following:

```typescript
const obj: { prop?: number }
console.log(obj.prop === undefined ? undefined : obj.prop.toFixed(2))
```

### The `instanceof` Operator

If you use the `instanceof` operator, TypeScript infers the type of the value as the type on the right side of the operator:

```typescript
const value: unknown = ...
if(value instanceof Date) {
  console.log('The type is `Date`')
}
```

### `Array.isArray()`

You can use the `Array.isArray()` function to check whether a value is an array:

```typescript
window.addEventListener('message', (event) => {
  if(Array.isArray(event.data)) {
    console.log('The type is `unknown[]`')
  }
})
```

This is preferred over `instanceof Array` which doesn't work across windows and frames.

### User Defined Type Guards (Type Predicates)

We saw that validation of objects generates a lot of boilerplate code. You could extract the code like this

```typescript
type Entity = {
  id: number
}
function isEntity(value: unknown): boolean {
  return typeof value === 'object' && value !== null && 'id' in value && typeof value.id === 'number'
}
```

However, if you use this in an if-statement, TypeScript can no longer infer the type of the value:

```typescript
const value: unknown = ...
if(isEntity(value)) {
  console.log('Id', value.id) // <-- Type Error, since 'id` doesn't exist on `unknown`
}
```

The reason is that the type signature of `isEntity` reveals nothing about the type guard. You can include a _user-defined type guard_ to fix this:

```typescript
type Entity = {
  id: number
}
function isEntity(value: unknown): value is Entity {
  return typeof value === 'object' && value !== null && 'id' in value && typeof value.id === 'number'
}
```

This function still returns a boolean, but if the return value is `true`, TypeScript infers that the type of `value` is `Entity`. The expression `value is Entity` is called a _type predicate_.

CAUTION: The type predicate does not need to match the inferred type in the function body: TypeScript will simply trust that the predicate is accurate. In the example above, we could have written `value is null`, and TypeScript wouldn't have generated an error. So whenever you create a user-defined type guard, include unit tests to ensure that the type guard is accurate.

### Discriminated/tagged unions & Pattern Matching

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

## Type Assertions

You will encounter scenarios where you want to initialize a value to `undefined`, but later reassign it to a different value:

```typescript
let user: undefined | User = undefined

// Later...
user = await fetchUser() // Returns a `User`
```

In this case, you must annotate `user` with a type `undefined | User`.

However, in some scenarios where you deal with records, you may have situation where you'd rather use the type inference to its greatest extent; for example, consider a state-like object:

```typescript
const state = {
  user: undefined,
  count: number
}
```

If most properties in the object can be inferred, it would be unnecessarily verbose to annotate it as such:

```typescript

const state: {
  user: User | undefined
  count: number
} = {
  user: undefined,
  count: number
}
```

To save yourself from excessive boilerplate, you can annotate the `user` property with the assertion operator (`as`):

```typescript
const state = {
  user: undefined as undefined | User,
  count: number
}
```

This tells TypeScript to infer `user` as `undefined | User`, instead of just `undefined`. You can also use it as an alternative to the type annotation separator (`:`):

```typescript
let user: undefined | User = undefined
// is equivalent to:
let user = undefined as undefined | User
```

NOTE: that nothing happens with the value on the left side--neither at runtime nor during compile time. When a TypeScript file is compiled into JavaScript, the type annotations are stripped, and you get simply:

You can only use type assertion (`as`) if the value on the left side is a subset of the type on the right side. The following are valid:

```typescript
// Correct ✅
const a = 1 as 1 | 2
const b = 100 as undefined | number
const c = undefined as undefined | number
```

But the following are incorrect:

```typescript
// Incorrect ❌
const a = 1 as 2 | 3
const b = 100 as undefined | string
const c = null as undefined | number
```

### Type Assertions with `unknown`

There is one exception to this rule: the `unknown` type. Even though the `unknown` type is the superset of all types, it can be asserted to any subtype. But this is mathematically incoherent, and it opens the door to a trick that lets you circumvent the type system: by asserting a type as `unknown`, you can then assert the unknown type as any other type without a type error:

```typescript
const id = '123' as unknown as number
```

Now, TypeScript will consider `id` as a number, when it in fact is a string! In some niche cases, it can be useful to override the type checker when you are absolutely certain that you know better than TypeScript. But needless to say, once you do this, TypeScript will no longer be able to save you from runtime errors. Use `as` with great caution!

## Functions: Parametric Values

While you're likely quite familiar with functions already, let's pause for a moment and think about what a function really is, mathematically speaking. This will be helpful when learning about generics.

A function can be though of a map from one value to another. To represent any function, simply write down a list of all inputs and the corresponding output; for example, the logical NOT can be represented as:

[cols="1,1"]
|===
| Input value | Output value
| `true`  | `false`
| `false` | `true`
|===

Since the input is of type `boolean`, there are only two possible inputs (`true` and `false`), and thus the table contains two rows. The type of this function is:

```typescript
// Not valid TypeScript
(boolean) => boolean
```

Unfortunately, https://github.com/microsoft/TypeScript/issues/13152#issuecomment-269099764[TypeScript requires] that the parameter has an arbitrary name–-even though it serves no purpose (other than documentation, possibly):

```typescript
// Correct
type Not = (a: boolean) => boolean
```

Functions with multiple arguments can be thought of functions with a single argument where the argument is a tuple; for example, the logical AND can be represented as:

[cols="1,1"]
|===
| Input value | Output value
| (`true, true`)  | `true`
| (`true, false`) | `false`
| (`false, true`) | `false`
| (`false, false`)| `false`
|===

Where the type of this function is:

```typescript
type And = (a: boolean, b: boolean) => boolean
```

In JavaScript, you _can_  implement functions as maps where the input-output pairs are key-value pairs. While the example above with the logical operations would be contrived, consider a more realistic scenario that maps color names to their RGB values:

```typescript
const colors = {
  red: [255, 0, 0],
  green: [0, 255, 0],
  blue: [0, 0, 255],
}
```

Now you can "call" this function by indexing:

```javascript
console.log(colors['red'])
```

To represent a function that takes a number as argument this way, you would need an object with 2^64^ properties, so instead, functions are normally represented as function expressions:

```javascript
const isPositive = (value) => value > 0
```

In TypeScript, you can annotate the identifier of a function like any value:

```typescript
type IsPositive = (value: number) => boolean
const isPositive: IsPositive = (value) => value > 0
```

Alternatively, annotate the parameters and the return type directly:

```typescript
const isPositive = (value: number): boolean => value > 0
```

A function can thus be thought of in two different ways:

1. As a value (object) that lists all input-output pairs.
2. As an expression that you need to hand a value (as input) before you get a value (as output) back.

## Generics: Parametric Types

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

### Generic Discriminated Unions

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

### Generic Function Types

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

## Type operators

In this chapter, you will learn some basic type operators. Although perhaps they're not the most exciting thing to learn about, they're simple; yet powerful; and can be used to construct complex types--especially when combined with mapped types.

### Intersection types (`&`)

We have frequently made use of the union type operator `|`, which corresponds to the set union operator stem:[\uu]. The intersection type operator `&` corresponds to the set intersection operator stem:[\nn], and returns the type where all values in the set exists in both type operands:

```typescript
type A = 1 | 2 | 3
type B = 2 | 3 | 4
// Result: `2 | 3`
type C = A & B
```

.The result of the intersection is a type that contain the values that exists in both sets; in this example `2 | 3`.
image:intersection.svg[]

There are two interesting special cases to consider--intersections with `never` and `unknown`:

- Since `never` represents an empty set, an intersection with `never` always results in `never`--there simply are no values that are both in a given type `T` _and_ the empty set `never`; for example;
+
```typescript
type A = 1 | 2 | 3
type B = never
// Result: `never`
type C = A & B
```
- Since `unknown` represents the set of all values, an intersection between a given type `T` and `unknown` always results in `T`--all values in `T` are also in `unknown`; for example;
+
```typescript
type A = 1 | 2 | 3
type B = unknown
// Result: `1 | 2 | 3`
type C = A & B
```

Intersections are most useful when dealing with objects. As an example, consider the following type:

```typescript
type Styleable = {
  color: string
  backgroundColor: string
}
```

It consists of _all_ objects where the `color` and `background` properties are `string`:

image:Stylable.svg[]

Note that this type does not forbid extra properties--the only requirement is that the `color` and `backgroundColor` properties have the type `string`, but it does not impose restrictions on other properties.

Now, consider a second type:

```typescript
type Clickable = {
  onClick: () => void
}
```

This type consists of all objects where the `onClick` property is a function that returns `void`. As with `Styleable`, it does not impose limitations on other properties:

image:Clickable.svg[]

When we take the intersection of `Styleable` and `Clickable`, we get the type of all objects that have `color`, `backgroundColor`, and `onClick` properties:

```typescript
// Result: { color: string, backgroundColor: strong, onClick: () => void }
type Button = Styleable & Clickable
```

image:StylableAndClickable.svg[]

But be mindful of using intersections on types that do not overlap. Let's start with a simple example:

```typescript
type EvenDigit = 0 | 2 | 4 | 6 | 8
type OddDigit = 1 | 3 | 5 | 7 | 9
// Result: `never`
type OddAndEvenDigit = Even & Odd
```

Which digits are both even and odd? Obviously, such number does not exist, so the set of both even and odd number is empty, which means that the type is `never`

image:intersection-never.svg[]

When performing intersections on object types, you're likely to at some point encounter scenarios where some properties resolve to `never`. Consider this intersection:

```typescript
type Button = {
     // Expects a string like '5px', or `50%`
    borderRadius: string
    onClick: () => void
}
type Box = {
    // Expects a number in pixels
    borderRadius: number
    padding: number
}
type BoxButton = Button & Box
```

This is equivalent evaluate to an object where each property value is an intersection:

```typescript
// Button & Box
type BoxButton = {
    // One of the operands are `unknown` because at least one of the types do not impose a constraint on the property
    // Result: () => void
    onClick: (() => void) & unknown
    // Result: number
    padding: unknown & number
    // Both operands impose constraint on `borderRadius`
    // Result: never
    borderRadius: string & number
}
```

Which values are both `number` and `string`? The answer is none, and thus the type corresponds to the empty set, which is represented by `never`.

```typescript
// Result
type BoxButton = {
    onClick: () => void
    padding: number
    borderRadius: never
}
```

In earlier chapters, we learned that it's not possible to construct a value from `never`, since there are no values to choose from:

```typescript
// Error: there's no value in `never` to assign to `a`.
const a: never
```

This implies that when a property is of type `never` we can't construct the object as a whole:

```typescript
// Error: the property `borderRadius` is missing, but we can't add it because there's no value in `never` to assign to it.
const boxButton: BoxButton = {
    onClick: () => undefined,
    padding: 10,
}
```

This means that the set of all values in `BoxButton` is empty, which means that `BoxButton` is effectively `never`.

### `typeof` type operator

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

### Indexed Access Types (`[]`)

You can index object types with the `[]` type operator to get the type of a property:

```typescript
type User = {
  id: number
  name: string
  email: string
}
type UserId = User['id'] // number
```

Just like with the `typeof` type operator, the `[]` type operator is evaluated at compile time and can only be used in type expressions. This means that the `'id'` type argument is not a string value, but a type.

This operator can be useful when you want to indicate that another property or function parameter has a relation to the property of an object type:

```typescript
type User = {
  id: number
  parentId: User['id']
}
const getUser = (id: User['id']) => {
  // ...
}
```

If it were not for the `[]` operator, you would only see `parentId: number` in the `User` type alias, and `id: number` in the function signature. Not that if you now change the type of `id` in `User`, the type of `parentId` and `id` in `getUser` will automatically be updated.

Indexed access types can be used with discriminated unions to extract the type that describes all possible tags in the union:

```typescript
type Asset = {
  type: 'image'
  url: string
} | {
  type: 'video'
  url: string
  duration: number
} | {
  type: 'audio'
  url: string
  duration: number
}
type AssetType = Asset['type'] // 'image' | 'video' | 'audio'
```

This is much more convenient than having to define the types twice.

It can also be used to index arrays:

```typescript
// as const so that we infer the tuple `['info', 'warn', 'error']` and not `string[]`
const allLogLevels = ['info', 'warn', 'error'] as const
// `typeof allColors` yields the type `['info', 'warn', 'error']`
type Color = (typeof allColors)[number] // 'info' | 'warn' | 'error'
```

Now all log levels are defined in one place, and you can easily add or remove log levels without having to update the type alias. There's no repetition of the words `'info'`, `'warn'`, and `'error'`.

### `keyof` type operator

While the indexed access type operator lets you extract the type of a property, the `keyof` type operator lets you extract the _keys_ of an object type:

```typescript
type PaletteColor = {
    main: string
    contrast: string
}
type Palette = {
    primary: PaletteColor
    secondary: PaletteColor
}
type Color = keyof PaletteColor
```

What type does `keyof Palette` evaluate to? Any value that is a key of `Palette` is either `'primary'` or `'secondary'`, which means that it is a union of these literal types--`'primary' | 'secondary'`.

```typescript
type Color = keyof Palette // 'primary' | 'secondary'
```

Here's an example of how it can come in handy:

```typescript
const palette: Palette = {
    primary: {
        main: 'deepskyblue',
        contrast: 'white',
    },
    secondary: {
        main: 'floralwhite',
        contrast: 'black',
    },
}

// color must be a key in the palette
const createButton = (color: keyof Palette) => {
    const button = document.createElement('button')
    button.style.backgroundColor = palette[color].main
    button.style.backgroundColor = palette[color].contrast
    return button
}

const primaryButton = createButton('primary')
const secondaryButton = createButton('secondary')
```

The benefit of deriving the type alias `Color` from the object type `Palette` is that the information about the keys are kept in one place--in `Palette`. But what if we want to do it the other way around; that is, to derive the type `Palette` from `Color`? Enter _mapped types_.

### Mapped Types

In this chapter, you will learn how to construct object and array types by mapping over the keys. Combined with generics, this lets us construct some very powerful higher-order types.

### Mapped Object Types

Forget about types for a moment and consider _values_: how can you construct an object from a set of keys? To be able to tie the solution into TypeScript, the solution should be written in a declarative, functional style. Here's a three-step plan:

1. Define a set of keys (as an array of string)
2. Map each key to a key-value pair (with `Array.prototype.map()]`)
3. Construct an object from all key-value pairs (with `Object.fromEntries()`)

Let's take a concrete example with vectors, where we want to construct an origin (all values zeros) from a set of dimensions:

```typescript
// 1) Define a set of keys
const dimensions = ['x', 'y', 'z']
// Result: { x: 0, y: 0, z: 0 }
const origin =
  // 3) construct an object from all key-value pairs
  Object.fromEntries(
    // 2) map each key to a key-value pair
    dimensions.map((dimension) =>
      [dimension, 0]
    )
  );
```

Now let's translate this question into TypeScript: how can we construct an object type from a set of keys? We'll use the same three-step plan:

1. Define a set of keys--in the type system, this is represented with a union of string literals
2. Map each key to a key-value pair (with a mapped type)
3. Construct an object from all key-value pairs (with an object type)

Let's consider the example with vectors, but this time we construct a type from the set of keys:

```typescript
// 1) Define a set of keys
type Dimension = 'x' | 'y' | 'z'
// Result: { x: number, y: number, z: number }
type CartesianVector3 =
// 3) construct an object from all key-value pairs
{
  // 2) map each key to a key-value pair
  [Key in Dimension]: number
}
```

Although the syntax differs a bit from JavaScript—there's no `map` or `fromEntries`—the principle is the same: the type is constructed by mapping over a set of keys.

Earlier, we saw how we could use the `keyof` operator to derive a type alias for the keys from an object type. We now know how to use mapped types to derive the object type from the keys:

```typescript
type Color = 'primary' | 'secondary'
type Palette = {
  [Key in Color]: {
      main: string
      contrast: string
  }
}
```

### Generic Mapped Types

In the previous example, `Key` is a type parameter—analogous to the argument in the callback function of `Array.prototype.map()`—and can be used on the right side of the colon to construct the object type:

```typescript
// Result: { a: 'a', b: 'b' }
type A = {
  [Key in 'a' | 'b']: Key
}
```

This is a silly example, but when used in parameterized (generic) types, `Key` can be used to _index the type parameter_.

```typescript
type Nullable<T> = {
  [Key in keyof T]: T[Key] | null
}
```

This parameterized type takes any object type `T`, maps each key `K` in `T` to the corresponding property value `T[K]`, and forms a union with `null`. The type that is returned is thus a variant of `T` where all properties are nullable; for example:

```typescript
type User = {
  id: number
  name: string
  email: string
}
// Result: { id: number | null, name: string | null, email: string | null }
type NullableUser = Nullable<User>
```

We can do the same with `undefined`:

```typescript
type Undefinable<T> = {
  [Key in keyof T]: T[Key] | undefined
}
```

### Optional Properties in Mapped Types

We saw how we can define a parameterized type that makes all properties into unions with `undefined`. But a property being `undefined` is not the same as being absent. For example, this is perfectly valid:

```typescript
// Ok: all properties are present
const user: Undefinable<User> = {
  id: 1,
  name: 'August',
  email: undefined,
}
```

but is not, for one of the properties is missing:

```typescript
// Error: `name` is missing
const user: Undefinable<User> = {
  id: 1,
  name: 'August',
}
```

To make properties _optional_ in mapped types, we can use the `?:` notation:

```typescript
type Partial<T> = {
  [Key in keyof T]?: T[Key]
}
```

This generic type takes any object type `T` and returns a new object type where all properties are optional.

Now we can write our code as such:

```typescript
// The email is missing, and that's okay!
// We can also assign `undefined` values to all properties
const user: Partial<User> = {
  id: 1,
  name: undefined,
}
```

To do the reverse--to make all properties required--use the `-?:` notation:

```typescript
type Required<T> = {
  [Key in keyof T]-?: T[Key]
}
```

TIP: TypeScript provides several so-called utility types to facilitate common type transformations. `Partial` and `Required` are two of those. You can use the utility types without fully understanding TypeScript generics. See the https://www.typescriptlang.org/docs/handbook/utility-types.html[full reference on typescriptlang.org]--they will come in handy!

### Mapped Types: Tuples

You can also map over tuples. Though we normally denote tuples and arrays with `[]`, TypeScript treats them as objects where the properties are numeric:

. Two equivalent ways to denote a tuple type
```typescript
type TripletA = [number, string, boolean]
type TripletB = {
  [0]: number
  [1]: string
  [2]: boolean
}
```

This means that you can create mapped types for tuples with the same syntax as for objects:

```typescript
type NullableTriplet<T> = {
  [Key in keyof T]: (value: T[Key]) => T[Key]
}
```

This utility type takes a tuple type `T` and returns a new tuple type where each element are transformations of the original elements.

## TODOs

Topics that were not covered, but which I intend to include in the future:

* `extends` type constraint
* `extends` conditional types, and `infer`
* covariance and contravariance
* `instanceof` type guard
* type declarations
  ** module augmentation
* `{}` type
* `object` type
* Compiler options and @typescript-eslint
* nominal vs structural type system.
* Exact and inexact object types (extra properties allowed)
** `satisfies`
* Testing generic types
  ** `@ts-expect-error` for testing generic types
* Index Signatures (`[key: string]: T`)
* Patterns to avoid
  ** `any`
  ** Enums
  ** Interfaces
  ** Assertion guards
  ** decorators
* Out of scope (short mention)
  ** template literals
  ** classes
  ** `this`
  ** `readonly`
* Common pitfalls
  ** Misuse of constants (use type literals)

## Appendix

Here are some additional topics that are not essential, and that do not fit well in the main text.

### Ignoring errors with `@ts-ignore`

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

```
TS2322: Type { [k: string]: number; } is not assignable to type { [key in Keys[number]]: number;
```

Which, if you look closely at the code, is actually an inaccurate error message.

However, this feature should be used with great caution. It not only forces you to outperform TypeScript in your analysis of the program, but it can severely compromise the maintainability of the code.

CAUTION: A good rule of thumb is to never use `@ts-ignore`.

TIP: Whenever you _do_ use `@ts-ignore``, ensure that you test the code thoroughly with automated tests.
