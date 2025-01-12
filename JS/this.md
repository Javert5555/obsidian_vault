# Определение
`this` — ключевое слово, которое является указателем на контекст в рамках которого оно применяется.

`this` функции объявленной через ключевое слово `function` - хранит ссылку на объект в котором эта функция была объявлена (если функция объявлена в глобальной области видимости, то у неё в `this` хранится ссылка на объект `Window`).

Функции объявленные через ключевое слово `function` доступны во всей области видимости (эффект всплытия) и во всех вложенных блоках.

## Если функция определена как стрелочная функция

```js
const arrowFunction = () => {  console.log(this);};
```

В этом случае значение `this` _всегда_ такое же, как у `this` в области действия родительской функции:

```js
const outerThis = this;const arrowFunction = () => {  // Всегда выводит `true`:  console.log(this === outerThis);};
```

Стрелочные функции хороши тем, что внутреннее значение `this` не изменяется: оно _всегда_ такое же, как у внешней функции (если стрелочная функция объявлена вне функции, то в `this` у неё всегда будет объект `Window`).

this стрелочной функции такой же как и у внешней функции, если внешней функции нет, то this = Window. 

```js
const fun6 = () => {
    console.log(this) // Window
}

function fun7 () {
    console.log(this) // Window
}

const obj = {
    fun1: function() {
        console.log(this) // obj
        function fun2() {
            console.log(this) // Window
        }
        fun2()
        const fun3 = () => {
            console.log(this) // obj
        }
        fun3()
        fun6()
    }
}

function fun4 () {
    console.log(this) // Window
}
fun4()

const fun5 = () => {
    console.log(this) // Window
}
fun5()
```

https://web.dev/i18n/ru/javascript-this/