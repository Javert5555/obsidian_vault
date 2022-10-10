# Spacing

Используйте хелпер theme.spacing(), чтобы создать постоянный интервал между элементами вашего пользовательского интерфейса.

MUI по умолчанию использует рекомендуемый коэффициент масштабирования 8 пикселей.

```js
const theme = createTheme();
theme.spacing(2); // `${8 * 2}px` = '16px'
```

## Custom spacing

Вы можете изменить преобразование интервала, предоставив:

-   число

```js
const theme = createTheme({
  spacing: 4,
});

theme.spacing(2); // `${4 * 2}px` = '8px'
```

-   функция

```js
const theme = createTheme({
  spacing: (factor) => `${0.25 * factor}rem`, // (Bootstrap strategy)
});

theme.spacing(2); // = 0.25 * 2rem = 0.5rem = 8px
```

-   массив

```js
const theme = createTheme({
  spacing: [0, 4, 8, 16, 32, 64],
});

theme.spacing(2); // = '8px'
```

## Multiple arity

Помощник `theme.spacing()` принимает до 4 аргументов. Вы можете использовать аргументы, чтобы уменьшить шаблон.

```diff
-padding: `${theme.spacing(1)} ${theme.spacing(2)}`, // '8px 16px'
+padding: theme.spacing(1, 2), // '8px 16px'
```

Также поддерживается смешивание строковых значений:

```js
margin: theme.spacing(1, 'auto'), // '8px auto'
```