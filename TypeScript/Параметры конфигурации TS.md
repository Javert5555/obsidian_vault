`strictNullChecks` - если `true`, то undefined и null рассматриваются, как отдельные типы, если `false`, то назначаются всем типам, что может привести к неожиданным ошибкам времени выполнения

Примеры, если `strictNullChecks: true`:

``` TS
const a : number = 1
const b : number = undefined // error TS2322: Type 'undefined' is not assignable to type 'number'
const c : number = null // error TS2322: Type 'null' is not assignable to type 'number'
```

Корректная работа:

``` TS
const a : number = 1
const b : undefined = undefined
const c : null = null

console.log(a, b, c) // 1 undefined null
```

___

Примеры, если `strictNullChecks: false:

``` TS
const a : number = 1
const b : number = undefined
const c : number = null

console.log(a, b, c) // 1 undefined null
```