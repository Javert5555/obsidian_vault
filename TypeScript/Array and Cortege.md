## Array

``` TypeScript
const arr1 : number[] = [1, 2, 3]
const arr2 : string[] = ['one', 'two', 'three']

const arr3 : (number | string)[] = [1, 'one', 2, 'two', 3, 'three']
```

Создание readonly массива
``` TypeScript
const arr4 : readonly string[] = ['one', 'two', 'three']
arr4.push('four') // error TS2339: Property 'push' does not exist on type 'readonly string[]'
arr4[0] = 'zero' // error TS2542: Index signature in type 'readonly string[]' only permits reading
```

Автоопределение типа
``` TypeScript
const arr5 = ['one', 'two', 'three']
arr5.push('four')
arr5.push(123) // error TS2345: Argument of type 'number' is not assignable to parameter of type 'string'
```

Декомпозиция массива
``` TypeScript
const arr6 : number[] = [1,2,3]
const [a, b] = arr6 // 1, 2
const [, c, d] = arr6 // 2, 3
const [e, ...f] = arr6 // 1, [2, 3]
```

Двумерный массив
``` TypeScript
const arr7 : number[][] = [[1, 2, 3], [4, 5, 6]]
arr7[2] = [7, 8, 9]
arr7.push([10, 11, 12])
console.log(arr7) // [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]]
```

Смешанные по типу массивы
``` TypeScript
const arr8 : (number | string)[] = [1, 'one', 2, 'two', 3, 'three']
// или
const arr9 : Array<number | string> = [1, 'one', 2, 'two', 3, 'three']
```

## Кортеж

``` TypeScript
const cortege1 : [string, number] = ['one', 1]
cortege1[0] = 'two'
console.log(cortege1) // ['two', 1]
```

Необязательный элемент в кортеже
``` TypeScript
const cortege2 : [string, number, number] = ['one', 1] // error TS2322: Type '[string, number]' is not assignable to type '[string, number, number]'.

const cortege2 : [string, number, number?] = ['one', 1]
```

При неопределённом количестве элементов
``` TypeScript
const cortege3 : [string, ...number[]] = ['one', 1, 2, 3]
```

Создание readonly кортежа
``` TypeScript
const cortege4 : readonly [string, ...number[]] = ['one', 1, 2, 3]
cortege4.push(5) // error TS2339: Property 'push' does not exist on type 'readonly [string, ...number[]]'.
```