`<BrowserRouter>` сохраняет текущее местоположение в адресной строке браузера, используя чистые URL-адреса, и осуществляет навигацию, используя встроенный стек истории браузера.
`BrowserRouter` — следует использовать когда вы обрабатываете на сервере динамические запросы.

`<BrowserRouter window>` по умолчанию использует текущий документ defaultView, но его также можно использовать для отслеживания изменений URL-адреса другого окна, например, в `<iframe>`.

``` jsx
import * as React from "react";
import { createRoot } from "react-dom/client";
import { BrowserRouter } from "react-router-dom";

const root = createRoot(document.getElementById("root"));

root.render(
  <BrowserRouter>
    {/* The rest of your app goes here */}
  </BrowserRouter>
);
```


https://habr.com/ru/post/329996/
https://reactdev.ru/libs/react-router/#_5
https://reactrouter.com/en/main/router-components/browser-router