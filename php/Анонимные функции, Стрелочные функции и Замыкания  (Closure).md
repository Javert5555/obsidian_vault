## Анонимные функции

Анонимные функции позволяют передавать в качестве параметров функции другие функции или присваивать их переменным. Анонимная функция определяется как обычная функция за тем исключением, что она не имеет имени.

```php
function foo(callable $fun) : void {
	$any = 'any';
	$fun($any);
}

$foo1 = function(mixed $something) { // анонимная функция
	var_export($something);
};

foo($foo1)
```

## Стрелочные функции

Стрелочные функции (arrow functions) в PHP - это сокращенная запись анонимных функций, которые возвращают значение. Они были введены в PHP 7.4.

Стрелочная функция определяется с помощью ключевого слова `fn` и оператора `=>` вместо ключевого слова `function` и фигурных скобок:

```php
fn(параметры) => выражение;
```

Основные особенности стрелочных функций в PHP:

- Автоматически возвращают значение выражения после `=>`, без использования ключевого слова `return`
- Имеют доступ к переменным из внешнего окружения (родительской области видимости и глобальной в том числе)
- Могут использовать в качестве параметров других функций
- Поддерживают type-hinting для параметров и возвращаемого значения[3]

Пример использования стрелочной функции:

```php
$closure = fn($a, $b) => $a + $b;
$result = $closure(5, 10); // 15
```

Это эквивалентно записи с использованием анонимной функции и `use`:

```php
$closure = function($a, $b) use($c, $d) {
    return $a + $b + $c + $d;
};
```
## Замыкания (Closure)

Замыкание в PHP - это анонимная функция, которая может использовать переменные из родительской области видимости с помощью ключевого слова `use`. Это позволяет функции "запоминать" переменные из места, где она была создана, и использовать их даже после завершения работы функции, которая их определила.

Вот пример замыкания в PHP:

```php
$greeting = "Hello";

$example = function($name) use ($greeting) {
    echo "$greeting, $name!";
};

$example("World"); // Выведет: Hello, World!
```

В этом примере анонимная функция, представленная переменной `$example`, использует переменную `$greeting` из внешней области видимости с помощью `use($greeting)`.

Замыкания полезны, когда функция определяется в одном месте, а используется в другом. Они позволяют не передавать множество переменных в качестве аргументов, а "захватывать" нужные из внешнего окружения.

Замыкания в PHP реализованы с помощью объектов класса `Closure`, которые хранят переданные параметры. Для вызова замыкания как функции используется магический метод `__invoke()`.

>[!info]
>Анонимная функция наследует переменную с тем значением, которое переменная содержала перед определением функции, а не в месте вызова функции:
>```php
$any = 'any';
$anonym = function(mixed $another) use($any) : void {
>	var_export($another . ' ' . $any);
};
$any = 'not any';
$anonym('something') // 'something any'
>```
>Чтобы всегда использовать актуальное значение переменной в момент вызова функции, можно использовать [[Ссылки|ссылки]] на значение переменной:
>```php
$any = 'any';
$anonym = function(mixed $another) use(&$any) : void { // используем ссылку &
>	var_export($another . ' ' . $any);
};
$any = 'not any';
$anonym('something') // 'something not any'
>```

_Наследование переменных из родительской области видимости отличается от наследования глобальных переменных. Глобальные переменные существуют в глобальной области видимости, которая остаётся прежней, какая бы функция ни выполнялась. Родительская область видимости замыкания — функция, в которой объявили замыкание; не обязательно функция, из которой замыкание вызвали._

>[!info]
Обычные функции могут наследовать переменные, только из глобальной области видимости, а анонимные функции (в том числе и стрелочные, которые являются сокращёнными версиями анонимных) могут наследовать переменные, как из глобальной области видимости, так и из родительской области видимости, например области видимости функции, в которой данная анонимная функция была объявлена. Например:
>```php
$global_var = 'global';
>
function foo() {
    global $global_var;
    var_export('foo '. $global_var); // 'foo global'
    $local_var = 'local';
>
    function foo1() {
        global $local_var, $global_var;
        var_export('foo1 ' . $local_var . ' ' . $global_var); // 'foo1 global'
    }
    foo1();
>
    $foo2 = function() use($local_var, $global_var) {
        var_export('foo2 ' . $local_var . ' ' . $global_var); // 'foo2 local global'
    };
    $foo2();
>
    $foo3 = fn() => 'foo3 ' . $local_var . ' ' . $global_var;
    var_export($foo3()); // 'foo3 local global'
}
foo();
>```
