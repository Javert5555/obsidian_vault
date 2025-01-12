Namespace - это пространство имён со своей областью видимости, которое предназначено для организации больших программ. Namespace позволяет группировать классы, интерфейсы, функции, переменные и другие пространства имён. 

```ts
namespace SpaceStation13 {
    type clown = {name: string, age: number}

    const rd: {name: string, role: string} = {
        name: 'name',
        role: 'bag guy'
    }

    function fn1(message: string): void {
        console.log(message)
    }

    class Security {
        constructor(
            public hos: string
        ) {}
        public getAlert(message: string): void {
            console.log(this.hos, message)
        }
    }

    namespace DepartmentsHeads  {
        type hos = {name: string}
        type hop = {name: string}
    }

}
```

Чтобы типы и объекты (любые сущности), определённые в пространстве имён, были видны извне, они определяются с ключевым словом export.

```ts
namespace SpaceStation13 {
    export type clown = {name: string, age: number}

    export const rd: {name: string, role: string} = {
        name: 'name',
        role: 'bag guy'
    }

    export function fn1(message: string): void {
        console.log(message)
    }

    export class Security {
        constructor(
            public hos: string
        ) {}
        public getAlert(message: string): void {
            console.log(this.hos, message)
        }
    }

    export namespace DepartmentsHeads  {
        type hos = {name: string}
        type hop = {name: string}
    }

    const inaccessibleVariable: string = 'inaccessible variable'

}

const newClown: SpaceStation13.clown = {
    name: 'flower',
    age: 25
}

SpaceStation13.fn1('hello') // hello 

SpaceStation13.inaccessibleVariable // error: Property 'inaccessibleVariable' does not exist on type 'typeof SpaceStation13'.ts(2339)
```

## Пространство имен в отдельном файле

Нередко пространства имен определяются в отдельных файлах. Например, определим файл Utils.ts со следующим кодом:

```ts
namespace Utils {
    export const key: string = '123'
    export const getPass = (name: string, age: number): string => `${name}:${age}`
}
```

И в той же папке определим главный файл приложения Customers.ts:

```ts
/// <reference path="./Utils.ts" />

const key = Utils.key
console.log(key) // 123
console.log(Utils.getPass('name', 33)) //name:33
```

С помощью директивы `/// <reference path="Utils.ts" />` подключается файл Utils.ts.

Далее нам надо объединить оба файла в один файл, который затем можно подключать на веб-страницу. Для этого при компиляции указывается опция:

```cmd
tsc -watch --outFile ./javascript/main.js main.ts Customers.ts Utils.ts
```

Опции outFile в качестве первого параметра передается название файла, который будет генерироваться. А последующие параметры - файлы с кодом TypeScript, которые будут компилироваться.

## Модули (legacy)

```ts
// Utils.ts
export const key: string = '123'
export const getPass = (name: string, age: number): string => `${name}:${age}`
```

```ts
// Customers.ts
import { getPass, key } from "./Utils"

console.log(key) // 123
console.log(getPass('name', 33)) //name:33
```

``` html
<script type="module" defer src="./javascript/Utils.js"></script>
<script type="module" defer src="./javascript/Customers.js"></script>
```