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
## Расширение псевдонимов

Одни псевдонимы могут расширять другие с помощью ключевого слова: `&`.

```TS
type obj1 = {name: string, age?: number}
type obj2 = obj1 & {email: string}

const newObj1: obj1 = {name: 'name'}
const newObj2: obj2 = {name: 'name', email: 'obj2@mail.com'}

const printObj = (obj: obj1) : void => {
    console.log(`${obj.name} ${obj?.age ? obj.age : ''}`)
}

printObj(newObj1)
printObj(newObj2)
```

 `newObj2` представляет так же тип obj1, поэтому ошибки нет, однако в таком случае, мы не можем использовать свойства и методы, неописанные в типе `obj1`:

```TS
type obj1 = {name: string, age?: number}
type obj2 = obj1 & {email: string}

const newObj1: obj1 = {name: 'name'}
const newObj2: obj2 = {name: 'name', email: 'obj2@mail.com'}

const printObj = (obj: obj1) : void => {
    console.log(`${obj.name} ${obj?.age ? obj.age : ''} ${obj.email}`) // error TS2339: Property 'email' does not exist on type 'obj1'.
}

printObj(newObj1)
printObj(newObj2)
```