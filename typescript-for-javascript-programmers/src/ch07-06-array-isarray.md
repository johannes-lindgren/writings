# Array.isArray()

You can use the `Array.isArray()` function to check whether a value is an array:

```typescript
window.addEventListener('message', (event) => {
  if(Array.isArray(event.data)) {
    console.log('The type is `unknown[]`')
  }
})
```

This is preferred over `instanceof Array` which doesn't work across windows and frames.