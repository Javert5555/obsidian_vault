	The Typography component makes it easy to apply a default set of font weights and sizes in your application.

![[Pasted image 20230131171609.png]]


## Изменение смыслового элемента  
  
Компонент Typography использует свойство variantMapping, чтобы связать вариант пользовательского интерфейса с семантическим элементом. Важно понимать, что стиль компонента типографики не зависит от семантического основного элемента.  
  
Вы можете изменить базовый элемент для одноразовой ситуации с помощью свойства `component`:
```jsx
<Typography variant="h1" component="h2">
  h1. Heading
</Typography>;
```

-   Можно изменить сопоставление глобально, используя тему:

```js
const theme = createTheme({
  components: {
    MuiTypography: {
      defaultProps: {
        variantMapping: {
          h1: 'h2',
          h2: 'h2',
          h3: 'h2',
          h4: 'h2',
          h5: 'h2',
          h6: 'h2',
          subtitle1: 'h2',
          subtitle2: 'h2',
          body1: 'span',
          body2: 'span',
        },
      },
    },
  },
});
```

## System props

Как служебный компонент CSS, Typography поддерживает все системные свойства. Вы можете использовать их как опору непосредственно в компоненте. Например, margin-top:

```jsx
<Typography mt={2}>
```