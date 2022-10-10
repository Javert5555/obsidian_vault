# Mongoose Virtuals

В Mongoose виртуальный объект — это свойство, которое не хранится в MongoDB. Виртуальные объекты обычно используются для вычисляемых свойств документов.

Функция Schema#virtual() возвращает объект VirtualType. В отличие от обычных свойств документа, виртуальные не имеют никакого базового значения, и Mongoose не выполняет приведение типов к виртуальным. Однако у виртуальных машин есть геттеры и сеттеры, что делает их идеальными для вычисляемых свойств.

```javascript
const userSchema = mongoose.Schema({
  firstName: String,
  lastName: String
});
// Создайте виртуальное свойство `fullName` с помощью геттера и сеттера.
userSchema.virtual('fullName').
  get(function() { return `${this.firstName} ${this.lastName}`; }).
  set(function(v) {
    // `v` это устанавливаемое значение, поэтому используйте
    // значение для установки `firstName` and `lastName`.
    const firstName = v.substring(0, v.indexOf(' '));
    const lastName = v.substring(v.indexOf(' ') + 1);
    this.set({ firstName, lastName });
  });
const User = mongoose.model('User', userSchema);

const doc = new User();
// Vanilla JavaScript триггерит сеттер
doc.fullName = 'Jean-Luc Picard';

doc.fullName; // 'Jean-Luc Picard'
doc.firstName; // 'Jean-Luc'
doc.lastName; // 'Picard'
```