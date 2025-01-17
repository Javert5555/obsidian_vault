# Schema

## Определение и формат

**Схема** в **Mongoose** определяет метаданные модели - ее свойства, типы данных и ряд другой информации.
```
const child = new Schema({ name: String });
const schema = new Schema({ name: String, age: Number, children: [child] });
const Tree = mongoose.model('Tree', schema);
```

## Определение типа

В качестве типа данных можно указывать одно из следующих значений:

-   String
-   Number
-   Date
-   Buffer
-   Boolean
-   Mixed
-   ObjectId
-   Array
-   Decimal128
-   Map

## Значение по умолчанию

С помощью параметра default мы можем указать значение по умолчанию для свойства.

```
const child = new Schema({
	name: {
		type: String,
		default: "John"
	}
});
```

### Валидация

Mongoose имеет ряд встроенных правил валидации, которые мы можем указать в схеме:

-   required: требует обязательного наличия значения для свойства
    
-   min и max: задают минимальное и максимальное значения для числовых данных
    
-   minlength и maxlength: задают минимальную и максимальную длину для строк
    
-   enum: строка должна представлять одно из значений в указанном массиве строк
    
-   match: строка должна соответствовать регулярному выражению
    
- unique: уникальность значения для свойства

```
const child = new Schema({
	name: { type: String, maxLength: 20, unique: true }
});
```

`model.isNew()`: проверьте, не сохранена ли модель на сервере.

## validate()
https://mongoosejs.com/docs/api.html#schematype_SchemaType-validate