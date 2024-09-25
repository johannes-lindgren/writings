# The `keyof` type operator


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

