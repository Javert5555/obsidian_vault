# Палитра
Палитра позволяет изменять цвет компонентов в соответствии с вашим брендом.

Тема раскрывает следующие цвета палитры (доступны в разделе theme.palette.):

- _primary_ - используется для представления основных элементов интерфейса для пользователя. Это цвет, который чаще всего отображается на экранах и компонентах вашего приложения.
- вторичный - используется для представления вторичных элементов интерфейса для пользователя. Он предоставляет больше способов подчеркнуть и выделить ваш продукт. Его наличие необязательно.
- _error_ - используется для обозначения элементов интерфейса, на которые пользователь должен обратить внимание.
- _warning_ - используется для представления потенциально опасных действий или важных сообщений.
- _info_ - используется для представления пользователю информации, которая является нейтральной и не обязательно важной.
- _success_ - используется для обозначения успешного завершения действия, которое было инициировано пользователем.

## Использование цветового объекта

Самый простой способ настроить цвет палитры - импортировать один или несколько предоставленных цветов и применить их:

```js
import { createTheme } from '@mui/material/styles';
import blue from '@mui/material/colors/blue';

const theme = createTheme({
  palette: {
    primary: blue,
  },
});
```

## Предоставление цветов напрямую

Если вы хотите предоставить более индивидуальные цвета, вы можете либо создать свой собственный цвет палитры, либо напрямую предоставить цвета некоторым или всем ключам `theme.palette`:

```js
import { createTheme } from '@mui/material/styles';

const theme = createTheme({
  palette: {
    primary: {
      // light: will be calculated from palette.primary.main,
      main: '#ff4400',
      // dark: will be calculated from palette.primary.main,
      // contrastText: will be calculated to contrast with palette.primary.main
    },
    secondary: {
      light: '#0066ff',
      main: '#0044ff',
      // dark: will be calculated from palette.secondary.main,
      contrastText: '#ffcc00',
    },
     // Provide every color token (light, main, dark, and contrastText) when using
     // custom colors for props in Material UI's components.
     // Then you will be able to use it like this: `<Button color="custom">`
     // (For TypeScript, you need to add module augmentation for the `custom` value)
    custom: {
      light: '#ffa726'
      main: '#f57c00',
      dark: '#ef6c00',
      contrastText: 'rgba(0, 0, 0, 0.87)',
    }
    // Used by `getContrastText()` to maximize the contrast between
    // the background and the text.
    contrastThreshold: 3,
    // Used by the functions below to shift a color's luminance by approximately
    // two indexes within its tonal palette.
    // E.g., shift from Red 500 to Red 300 or Red 700.
    tonalOffset: 0.2,
  },
});
```