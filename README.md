Шпаргалка по ECMAScript 2019 и React

## ECMAScript 2019

### Параметры по умолчанию

Если запустить функцию без параметров или с одним из них, другие возьмут значения по умолчанию:
```javascript
function testGo(count = 10, start = 4) {
    console.log(count, start);
}

testGo(); // 10, 4
```
Параметры по умолчанию ставят последними

### Rest параметр

Позволяет работать с переменным количеством аргументов: 
```javascript
function max(...numbers) {
  console.log(numbers);
}

max('text', 1, 17); // ['text', 1, 17]
max(5); // [5]
max(); // []
```
Собирает все аргументы в массив

### Spread оператор

Раскладывает массив на список независимых элементов:
```javascript
const arr1 = [1, 4, 8];
const arr2 = [3, 6, 5];

const res = Math.max(...arr1, 5, ...arr2, 12);

console.log(res); // 12
```
Можно комбинировать с обычными аргументами

Можно создать копию массива(новый массив):
```javascript
const arr1 = [1, 4, 8];
const arr2 = [3, 6, 5];

const shallowCopy = [...arr1, ...arr2, 41];

console.log(shallowCopy); // [1, 4, 8, 3, 6, 5, 41]
```

### Деструктуризация объектов

Позволяет вытащить сразу несколько переменных из объекта, вместо того, чтобы делать это с каждой:
```javascript
const person = {
  name: {
    first: 'Peter',
    last: 'Smith',
  },
  age: 28
};

const {firstName, lastName} = person;

console.log(firstName, lastName); // Peter Smith
```

Если нужные значения лежат внутри вложенного объекта:
```javascript
const person = {
  name: {
    first: 'Peter',
    last: 'Smith',
  },
  age: 28
};

const {name: {first: firstName, last: lastName}} = person;

console.log(firstName, lastName); // Peter Smith
```
Здесь сразу помещаем значения в новые переменные firstName и lastName.

Можно задать значение по умолчанию, если его не окажется в объекте:
```javascript
const person = {
  name: {
    first: 'Peter',
    last: 'Smith',
  },
  age: 28
};

const {role = 'user'} = person;

console.log(role); // user
```