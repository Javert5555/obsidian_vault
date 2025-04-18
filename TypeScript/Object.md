Пример:

``` TS
const obj1 : {name: string; age: number} = {
    name: 'name1',
    age: 44,
}
console.log(obj1) // { name: "name1", age: 44 }
obj1.name = 'name2'
obj.age = 22

console.log(obj1) // { name: "name2", age: 22 }

obj.age = 'old' // error: Type 'string' is not assignable to type 'number'
```

Нельзя добавлять свойства, неопределённые в типе переменной:

```ts
obj1.email = 'email@mail.com' // error: Property 'email' does not exist on type '{ name: string; age: number; }'
```

## Необязательные свойства:

TypeScript позволяет сделать свойства необязательными. Для этого после названия свойства указывается знак вопроса `?`:

```ts
const obj2 : {name: string; age?: number} = {
    name: 'name2'
}
console.log(obj2) // { name: "name2" }

console.log(obj2.age) // undefined
console.log(obj2.email) // error : Property 'email' does not exist on type '{ name: string; age?: number; }'

obj2.age = 44
console.log(obj2) // { name: "name3", age: 44 }
```

Если в типах объекта указан необязательное свойство (с помощью вопросительного знака), то это свойство должно быть указано как необязательное в параметре функции, которой передаётся этот объект:

```ts
// пример на свойстве age
const obj3 : {name: string; age?: number, email: string} = {
    name: 'name3',
    age: 33,
    email: 'email@mail.com',
}

function fn2({name, age} : {name: string; age: number}) : void {
    console.log(name, age)
}
fn2(obj3) // error: Argument of type '{ name: string; age?: number; email: string; }' is not assignable to parameter of type '{ name: string; age: number; }'.Property 'age' is optional in type '{ name: string; age?: number; email: string; }' but required in type '{ name: string; age: number; }'.

function fn3({name, age} : {name: string; age?: number}) : void {
    console.log(name, age)
}
fn2(obj3) // name3 33
```

При обращении к неустановленному необязательному свойству мы получим undefined, а при обращении к несуществующему свойству получим ошибку:

```TS
type obj1 = {name: string, age?: number}
const newObj : obj1 = {name: 'name'}

console.log(newObj.name) // name
console.log(newObj.age) // undefined
console.log(newObj.status) // error TS2339: Property 'status' does not exist on type 'obj1'
```
## Проверка наличия свойств:

```ts
const obj2 : {name: string; age?: number} = {
    name: 'name2'
}

console.log(obj2.test) // error: Property 'test' does not exist on type '{ name: string; age?: number; }'

if ('test' in obj2) {
    console.log(obj2.test)
} else {
    console.log('no')
}
```

## Объект в качестве аргумента функции:

```ts
const obj2 : {name: string; age?: number} = {
    name: 'name2'
}
obj2.age = 55

function fn1(some_obj: {name: string; age?: number}) : void {
    console.log(some_obj.age)
}

fn1(obj2)
```

Работает даже, если в объекте есть лишнее свойство:

```ts
// 
const obj3 : {name: string; age?: number, email: string} = {
    name: 'name3',
    age: 33,
    email: 'email@mail.com',
}

fn1(obj3)
```

Но, если передать объект напрямую, то будет ошибка:

```ts
fn1({
    name: 'name3',
    age: 33,
    email: 'email@mail.com', //error: Object literal may only specify known properties, and 'email' does not exist in type '{ name: string; age?: number; }'.ts(2353)
})
```

## Декомпозиция

``` ts
// пример на свойстве age
const obj3 : {name: string; age?: number, email: string} = {
    name: 'name3',
    age: 33,
    email: 'email@mail.com',
}

function fn3({name, age} : {name: string; age?: number}) : void {
    console.log(name, age)
}
fn2(obj3) // name3 33
```

Можно вынести тип объекта с помощью ключевого слова `type`:

