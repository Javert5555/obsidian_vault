Type assertion представляет модель преобразования значения переменной к определённому типу. В определённых случаях переменная может представлять тип `any` или `union`, в то время как нам необходимо использовать переменную, как значение строгого типа. Для этого можно привести переменную к этому типу.

Пример:

```TS
const header = document.querySelector('#header')

header.innerText = 'TS' // 'header' is possibly 'null' ts(18047)
```

Приведём переменную `header` к типу `HTMLElement`. Так мы получим объект типа `HTMLElement`.

Первый способ:

``` TS
const header = <HTMLElement>document.querySelector('#header')

header.innerText = 'TS' // Ошибок нет
```

Второй способ:

``` TS
const header = document.querySelector('#header') as HTMLElement

header.innerText = 'TS' // Ошибок нет
```

>[!main] Замечание
>Если приведение к такому типу невозможно, то во время выполнения кода мы получчим ошибку.

