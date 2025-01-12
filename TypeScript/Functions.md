## Базовый синтаксис

```ts
const fn1 = (name: string, age: number | string) : number | string => {
    console.log(name)
    return age
}

function fn2(name: string, age: number | string): number | string {
    console.log(name)
    return age
}

const fn3 = function(name: string, age: number | string): number | string {
    console.log(name)
    return age
}
```

## Значения по умолчанию

```ts
const fn1 = (name: string = "name", age: number | string = 18) : number | string => {
    console.log(name)
    return age
}
```

## Необязательный (опциональный) аргумент

Опциональный аргумент обозначается при помощи оператора `?` после аргумента:
```ts
const fn1 = (name: string = "name", age: number | string = 18, email?: string) : number | string => {
    console.log(name)
    return age
}
```

## Rest type (остаточные параметры)

```ts
const newFn = (name: string, ...args: (string | number)[]): void => {
    console.log(`${name}, ${args.join(', ')}`)
}

newFn('name', 33, 'test@mail.com') // name, 33, test@mail.com
```

## Never and Void

Если функция ничего не возвращает, то указываем тип `void`:

```ts
const newFn = (): void => {
    console.log('text')
}
```

Если функция возвращает ошибку или выполняется бесконечное количество времени, то указываем тип `never`:

```ts
const errFn = (): never => {
    throw new Error('error')
}
errFn() // Error: error

const infinityFn = (): never => {
    while(true) {
        console.log(123)
    }
}
infinityFn() // 123
```

## Тип функции

```ts
let mainFn: (firstArg: string) => void // описываем тип функции

function oldFn(name: string): void {
    console.log(name)
}

mainFn = oldFn
mainFn('name')
```

Если попробовать присвоить переменной функцию другого типа, то получим ошибку

```ts
let mainFn: (firstArg: string) => string // описываем тип функции

function oldFn(name: string): void {
    console.log(name)
}

mainFn = oldFn // error: Type '(name: string) => void' is not assignable to type '(firstArg: string) => string'.
  Type 'void' is not assignable to type 'string'.
mainFn('name')
```