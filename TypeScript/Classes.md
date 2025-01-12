## Объявление класса:

```ts
class Button {
    text: string;
    className: string[];
}

const btn1 = new Button()
btn1.text = 'btn text'
btn1.className = ['btn', 'red-btns']
console.log(btn1) // Object { text: "btn text", className: ["btn", "red-btns"] }
```

Можно сразу задать значения для свойств экземпляра класса:
```ts
class Header {
    text : string = 'main header'
    fontSize : string = '32px'
}
const h1 = new Header()

console.log(h1) // Object { text: "main header", fontSize: "32px" }
```

## Метода класса

Методы - функции внутри класса или экземпляра класса:

``` ts
class Btn {

    text: string
    className: string[]
    
    create(): HTMLButtonElement {
        const btn = document.createElement('button')
        btn.textContent = this.text
        this.className.forEach(el => {
            btn.classList.add(el)
        })
        return btn
    }
}

const btn2 = new Btn()

console.log(btn2)
btn2.text = 'btn2'
btn2.className = ['btn', 'red-btns']
document.body.append(btn2.create())
```

## Конструктор класса

```ts
class BtnEl {
    text: string
    className: string[]
    constructor(text : string, className : string[]) {
        this.text = text
        this.className = className
    }

    create(): HTMLButtonElement {
        const btn = document.createElement('button')
        btn.textContent = this.text
        this.className.forEach(el => {
            btn.classList.add(el)
        })
        return btn
    }
}

const btn3 = new BtnEl('btn3', ['btn', 'red-btns'])
console.log(btn3)
document.body.append(btn3.create())
```

## Readonly свойства

```ts
class BtnEl {
    readonly role: string
    // можно и так
    readonly type: string = 'main button'
    constructor() {
        this.role = 'main button'
    }
}

const btn5 = new BtnEl()
btn5.role = 'just button' // error: Cannot assign to 'role' because it is a read-only property.
```

## Наследование

Базовый синтаксис
```ts
class C1 {
    name : string = 'name1'
}

class C2 extends C1 {
    age : number = 33
}

class C3 extends C2 {
    showData(): void {
        console.log(`${this.name}, ${this.age}`)
    }
}

const obj1 = new C3()
obj1.showData() // name1 33
```

Использование функции constructor для инициализации свойств класса.

```ts
class C4 extends C1 {
    age: number = 33
    constructor(name: string, age: number) {
        super()
        this.name = name
        this.age = age
    }
    showData(): void {
        console.log(`${this.name}, ${this.age}`)
    }
    showInfo(): string {
        return `${this.name} ${this.age}`
    }
}

const obj2 = new C4('name4', 44)
obj2.showData()
```

Функция `super` вызывается в конструкторе класса, которые наследуется от другого класса, чтобы вызвать конструктор наследуемого класса (чтобы проинициализировать свойства базового класса).

## Наследование с изменением конструктора

```ts
class C5 extends C4 {
    email: string = 'default@mail.com'
    constructor(name: string, age: number, email: string) {
        super(name, age) // передаём свойства, которые попадают в функцию constructor родителя
    }
    // полное затирание функции родителя
    showData(): void {
        console.log(`${this.name}, ${this.age}, ${this.email}`)
    }
    // или можно дополнить функцию наследуемого класса
    showData(): void {
        super.showData() //
        console.log(`, ${this.email}`)
    }
    showInfo(): string {
        // сохраняем в переменную результат вызова функции showInfo наследуемого класса
        const str : string = super.showInfo()
        return `${str} ${this.email}`

    }
}

const obj3 = new C5('name5', 22, 'test@mail.com')
obj3.showData() // name5, 22, test@mail.com
console.log(obj3.showInfo()) // name5 22 test@mail.com
```

## Модификаторы доступа

Модификаторы позволяют скрыть состояние объекта от внешнего доступа и управлять доступом к этому состоянию. В TS существуют 3 модификатора состояния доступа: public, private, protected.

Если свойству или методу класса явно не задан модификатор доступа, то он по умолчанию устанавливается как public.

```ts
class C1 {
    public name: string // эквивалентно записи name: string
    age: number
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
    callAnything(): void {
        console.log(this.name, this.age)
    }
}

const obj1 = new C1('name', 22)
obj1.callAnything() // name 22
console.log(obj1.name) // name
```

Если к свойству или методу класса применяется модификатор private, то данное свойство или метод не будет доступен извне объекта (экземпляра) данного класса.

```ts
class C1 {
    private _name: string
    age: number
    constructor(name: string, age: number) {
        this._name = name
        this.age = age
    }
    callAnything(): void {
        console.log(this._name, this.age)
    }
    private privateMethod(): void {
        console.log('private')
    }
}

const obj1 = new C1('name', 22)
obj1.callAnything() // name 22
obj1.privateMethod() //error: Property 'privateMethod' is private and only accessible within class 'C1'
console.log(obj1.name) // error: Property 'name' is private and only accessible within class 'C1'
```

Если к свойству или методу класса применяется модификатор protected, то данное свойство или метод доступен извне класса только в классах-наследниках.

```ts
class C2 extends C1 {
    readonly email: string
    constructor(email: string, name: string, age: number) {
        super(name, age)
        this.email = email
    }
    sayAnything2(): void {
        console.log(this.email)
        // свойство age видно внутри класса наследника
        console.log(this.age)
        // так как к свойству name применен модификатор private его не видно внутри класса наслендика
        console.log(this.name) // error: Property 'name' does not exist on type 'C2'.
    }
}

const obj = new C2('email', 'name', 33)
obj.sayAnything2() // email
                  // 33
obj.age // error: Property 'age' is protected and only accessible within class 'C1' and its subclasses
```

Если к свойству или методу класса применяется модификатор readonly, то данное свойство или метод доступен извне класса только для чтения.

```ts
class C2 extends C1 {
    readonly email: string
    constructor(email: string, name: string, age: number) {
        super(name, age)
        this.email = email
    }
    sayAnything2(): void {
        console.log(this.email)
        // свойство age видно внутри класса наследника
        console.log(this.age)
        // так как к свойству name применен модификатор private его не видно внутри класса наслендика
        console.log(this.name) // error: Property 'name' does not exist on type 'C2'.
    }
}

const obj = new C2('email', 'name', 33)
console.log(obj.email) // email
obj.email = 'something' // error: Cannot assign to 'email' because it is a read-only property
```

## Определение полей через конструктор

```ts
class C1 {
    private name: string
    protected age: number
    constructor(name: string, age: number) {
        this._name = name
        this.age = age
    }
    callAnything(): void {
        console.log(this._name, this.age)
    }
}

const obj1 = new C21('name1', 22)
obj1.callAnything() // name1 22
```

Этот класс аналогичен следующему, так как использование модификаторов доступа в параметрах конструктора свойства создаются автоматически и нам не нужно объявлять их явно (модификаторов доступа обязателен для всех параметров, даже для публичных):

```ts
class C1 {
    constructor(
        private name: string,
        protected age: number,
        public email: string
    ) {}
    callAnything(): void {
        console.log(this.name, this.age, this.email)
    }
}

const obj1 = new C1('name1', 22, 'test@mail.com')
obj1.callAnything() // name1 22 test@mail.com
```

## Аксессоры Get/Set

Get и Set - это методы класса, которые ведут себя снаружи класса как свойства и служат либо для чтения, либо для установки значения внутри него.

```ts
class User {
    private _age: number = 20
    constructor(
        public name: string
    ) {}
    
    setAge(age: number): void {
        this._age = age
    }

    public set setMyAge(age: number) {
        this._age = age
    }

    public getAge(): number {
        return this._age
    }

    public get getMyAge(): number {
        return this._age;
    }
}

const user1 = new User('name')

user1._age = 29 // error: Property '_age' is private and only accessible within class 'User'
user1.setAge(30) // 30
user1.setMyAge = 31 // 31
console.log(user1.getAge()) // 31
console.log(user1.getMyAge) // 31
```

## Статические свойства и методы класса

Для обращения к статическому методу или свойству применяется имя класса, а не экземпляра класса. Статические свойства и методы объявляются с помощью ключевого слова `static`:
```ts
class User {
    static secret: number = 30
    constructor(
        public name: string,
        public age: number
    ) {}
    getInfo(): void {
        console.log(`${this.name} ${this.age} ${User.secret}`)
    }
}

const user1 = new User('name', 20)
user1.getInfo() // name 20 30
console.log(User.secret) // 30
user1.secret // error: Property 'secret' does not exist on type 'User'. Did you mean to access the static member 'User.secret' instead?
```

Класс наследник унаследует статическое свойство:

```ts
class User {
    private nickname: string = 'username'
    static secret: number = 30
    constructor(
        public name: string,
        public age: number
    ) {}
    getInfo(): void {
        console.log(`${this.name} ${this.age} ${User.secret}`)
    }
}

class SecondUser extends User {
    public name: string = 'user2'
    constructor(age: number, name?: string) {
        super(name, age)
    }
}

const user1 = new User('name', 20)
const user2 = new SecondUser(20)
console.log(SecondUser.secret) // 30
user2.getInfo() // user2 20 30
```

## Абстрактные классы (абстрактные методы)

Абстрактные классы - это базовые классы, от которых наследуются другие классы. От абстрактного класса нельзя создать экземпляр (объект) этого класса.

```ts
abstract class User {
    static secret: number = 30
    constructor(
        public name: string,
        public age: number
    ) {}
    getInfo(): void {
        console.log(`${this.name} ${this.age} ${User.secret}`)
    }
}

class SecondUser extends User {
    public name: string = 'nick'
    constructor(age: number, name?: string) {
        super(name, age)
    }
}

const user1 = new User('name', 20) // error: Cannot create an instance of an abstract class
const user2 = new SecondUser(20)
console.log(SecondUser.secret) // 30
user2.getInfo() // user2 20 30
```

Абстрактные методы обязательно должны быть описаны в потомке, иначе компилятор выдаст ошибку:

```ts
abstract class User {
    static secret: number = 30
    constructor(
        public name: string,
        public age: number
    ) {}
    getInfo(): void {
        console.log(`${this.name} ${this.age} ${User.secret}`)
    }
    abstract greet(): void
}

class SecondUser extends User {
    public name: string = 'nick'
    constructor(age: number, name?: string) {
        super(name, age)
    }
    greet(): void {
        console.log('greetings')
    }
}

const user2 = new SecondUser(20)
console.log(SecondUser.secret) // 30
user2.getInfo() // user2 20 30
```

