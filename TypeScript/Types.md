
>[!info]
>В TypeScript существует 10 следующих базовых типов:
>1. string - строка
>2. number - целые числа и числа с плавающей точкой
>3. bigint - целых чисел больше 2^53 - 1
>4. boolean - логическое значение true или false
>5. `[ ]` (array) - массив
>6. кортежи - (массивы с разными типами), просто кортежи
>7. any - произвольный тип
>8. symbol - символьное значение
>9. enum - перечисления
>10. never - указывает на отсутствие  значения, используется в качестве возвращаемого типа функций, которые генерируют или возвращают ошибку
## String

``` TypeScript
const str : string = 'string one'
```

## Number

``` TypeScript
const num1 : number = 123
const num2 : number = 12.3
...
```

## BigInt

``` TypeScript
const bigintNum1 : bigint = BigInt(100)
const bigintNum2 : bigint = 100n
...
```

## Boolean

``` TypeScript
const isTrue : boolean = true
const isFalse : boolean = false
```

## Array

``` TypeScript
const arr1 : number[] = [1, 2, 3]
const arr2 : string[] = ['one', 'two', 'three']

const arr3 : (number | string)[] = [1, 'one', 2, 'two', 3, 'three']
```
## Кортеж

``` TypeScript
const cortege1 : [string, number] = ['one', 1]
```
## Any

``` TypeScript
const any1 : any = 'str'
const any2 : any = 123
const any3 : any = [1, 'asd', true]
const any4 : any = {name: 'name', age:212}
```

## Symbol

``` TypeScript
const symb : symbol = Symbol('symb')
```

## Enum

enum или перечисление позволяет определить набор именнованных констант, которые описывают определенные состояния.

``` TypeScript
enum Season1 { Winter, Spring, Summer, Autumn }

const currentSeason1 : Season1 = Season1.Summer // 2
const currentSeason2 : string = Season1[2] // 'Summer'

enum Season2 {
    Winter = 'Зима',
    Spring = 'Осень',
    Summer = 'Лето',
    Autumn = 'Осень',
}

const currentSeason3 : Season2 = Season2.Winter // Зима
const currentSeason4 : Season2 = Season2[0] // undefined

enum Season3 {
    Winter = 1,
    Spring = 'Весна',
    Summer = 3,
    Autumn = 'Осень'
}

const currentSeason5 : Season3 = Season3.Summer // 3
const currentSeason6 : Season3 = Season3.Autumn // Осень
```

При константном перечислении, в файле JS не будет создаваться объект с перечислением:
```ts
const enum links {
	youtube: 'https://www.youtube.com',
	vk: 'https://www.vk.com',
	telegram: 'https://www.telegram.com',
}

const arr = [links.vk, links.youtube]

// скомпилированный js файл
"use strict"
const arr = ["https://www.vk.com" /* vk */, "https://www.youtube.com" /* youtube */]

```

## Never

never — это примитивный тип, который олицетворяет собой признак для значений, которых никогда не будет. Или, признак для функций, которые никогда не вернут значения, то ли по причине ее зацикленности, например, бесконечный цикл, то ли по причине ее прерывания.

``` TypeScript
/** Пример с прерыванием */
function error(message: string): never {
    throw new Error(message);
}

/** Бесконечный цикл */
function infiniteLoop(): never {
    while (true) {
    }
}

/** Божественная рекурсия */
function infiniteRec(): never {
    return infiniteRec();
}
```

## Составные типы

``` TypeScript
const a : number | string = 'asd'
const b : number | string = 1

const arr1 : (number | string)[] = [1, 'one', 2, 'two', 3, 'three']

const arr2 : {name: string, age: number}[] = [
    {name: 'name1', age: 40},
    {name: 'name2', age: 30},
]

```

## Ключевое слово type

Если есть повторяющиеся сложные  тип (например тип объекта), можно использовать ключевое слово `type`, чтобы не нарушать принцип DRY:

```TS
const obj1 : {name: string, age: number} = {
    name: 'name1',
    age: 111,
}

const obj2 : {name: string, age: number} = {
    name: 'name2',
    age: 222,
}

const obj3 : {name: string, age: number} = {
    name: 'name3',
    age: 333,
}
```

 Вынесем тип с помощью `type`:
 
```ts
type objectsType = {name: string, age: number}

const obj1 : objectsType = {
    name: 'name1',
    age: 111,
} 

const obj2 : objectsType = {
    name: 'name2',
    age: 222,
}

const obj3 : objectsType = {
    name: 'name3',
    age: 333,
}
```

Если объекты немного отличаются можно модифицировать уже существующий тип:

```ts
type objectsType = {
    name: string,
    age: number,
    nickname?: string,
    email?: string,
    getPassword?: () => void,
}

const obj1 : objectsType = {
    name: 'name1',
    age: 111,
    nickname: 'nickname1',
}

const obj2 : objectsType = {
    name: 'name2',
    age: 222,
    email: 'test@mail.com'
}

const obj3 : objectsType = {
    name: 'name3',
    age: 333,
    getPassword(): void {
        console.log(123)
    }
}
```