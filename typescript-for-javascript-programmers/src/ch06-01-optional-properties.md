# Optional Properties

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
