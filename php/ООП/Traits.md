
Traits представляют группу методов, которые могут быть добавлены в классы. Traits позволяют определять блоки функционала и многократно повторно использовать в классах без необходимости усложнять код классов, которые используют эти методы.

(traits) в PHP — это механизм, введенный в версии 5.4, который позволяет разработчикам повторно использовать код в классах без необходимости создания сложной иерархии наследования. Они предназначены для решения проблем, связанных с единственным наследованием, позволяя использовать наборы методов в нескольких независимых классах.

>[!info]
>Трейты в PHP могут содержать статические, абстрактные и обычные методы методы.

Трейт определяется с помощью ключевого слова `trait`, за которым следует имя трейта. Для применения классов трейта применяется оператор `use`, после которого указывается добавляемый трейт:

```php
trait Person {
	public function sayHello() {
		var_export($this->name);
	}
}

class Employee {
	use Person;
	
	public function __construct(
		private string $name = 'name',
	) {}
}

$new_empl = new Employee('emp');
$new_empl->sayHello(); // 'emp'
```

После добавления трейта с помощью оператора `use` класс может использовать его методы, как-будто они определены в самом этом классе.

**Трейты в PHP не переопределяют методы самого класса:**

```php
trait Person {
	public function sayHello() {
		var_export($this->name);
	}
}

class Employee {
	use Person

	public function __construct(
		private string $name = 'name',
	) {}

	public function sayHello() {
		var_export('hi');
	}
}

$new_empl = new Employee('emp');
$new_empl->sayHello(); // 'hi'
```

Следует учитывать, что при наследовании методы трейта переопределяют унаследованные методы с тем же именем:

```php
trait Person {
    public function sayHello() {
        echo "$this->name trait\n";
    }
}

abstract class People {
    protected string $name;

    public function sayHello() {
        echo $this->name;
    }
}

class Employee extends People {
    use Person {}

    function __construct(
        public string $name
    ) {}
}

$empl1 = new Employee('alex');
$empl1->sayHello(); // alex trait
```

Приватные методы trait будут доступны в классе:

```php
trait Person
{
    private function sayHello() {
        echo "$this->name trait\n";
    }
}  

class Employee extends People {
    use Person {}

    function __construct(

        public string $name

    ) {}

    function getName() {
        $this->sayHello();
    }
}

$empl1 = new Employee('alex');
$empl1->getName(); // alex trait
```