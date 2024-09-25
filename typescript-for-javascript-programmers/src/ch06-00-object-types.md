# Object Types (with named properties)

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
