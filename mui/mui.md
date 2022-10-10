Рекомендуемые API стилей в версии 5: `styled()`/`sx prop`. Если вы используете `styled()`, вы можете отделить код стиля, создав повторно используемый стилизованный компонент. `sx prop` больше подходит для встроенного одноразового стиля, поэтому это не лучший выбор для использования в этом сценарии:
```jsx
import * as React from 'react';
import Slider from '@mui/material/Slider';
import { alpha, styled } from '@mui/material/styles';

const SuccessSlider = styled(Slider)(({ theme }) => ({
  width: 300,
  color: theme.palette.success.main,
  '& .MuiSlider-thumb': {
    '&:hover, &.Mui-focusVisible': {
      boxShadow: `0px 0px 0px 8px ${alpha(theme.palette.success.main, 0.16)}`,
    },
    '&.Mui-active': {
      boxShadow: `0px 0px 0px 14px ${alpha(theme.palette.success.main, 0.16)}`,
    },
  },
}));

export default function StyledCustomization() {
  return <SuccessSlider defaultValue={30} />;
}
```
or
```jsx
<Slider
  defaultValue={30}
  sx={{
    width: 300,
    color: 'success.main',
  }}
/>
```

Кроме того, вы также можете использовать `variant` в MUI v5. Это работает следующим образом: вы создаете пользовательские стили и назначаете им имя, называемое `variant`, поэтому вместо указания `className`, как раньше, вы устанавливаете свойство `variant` следующим образом:

```javascript
<Button variant="myCustomVariant">
  Button
</Button>
```

Пользовательский вариант можно создать с помощью createTheme. Смотрите этот ответ для более подробной информации. Имейте в виду, что на данный момент эта функция поддерживается не во всех компонентах:

```javascript
const theme = createTheme({
  components: {
    MuiButton: {
      variants: [
        {
          props: { variant: 'myCustomVariant' },
          style: {
            textTransform: 'none',
            border: `2px dashed grey${blue[500]}`,
          },
        },
      ],
    },
  },
});
```

https://mui.com/material-ui/customization/how-to-customize/