# Tuples

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
