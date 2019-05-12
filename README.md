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

Можно задать значение по умолчанию у вложенного объекта (permissions), но нужно сделать проверку, если такого объекта нет - ставим ему значение по умолчанию (пустой объект):
```javascript
const {permissions: {role = 'user'} = {}} = person;
```

Деструктуризация аргументов функции
Вместо того, чтобы проверять параметры, задаем их по умолчанию:
```javascript
function connect({
  host = 'localhost',
  port = 2345,
  user = 'guest' } = {}) {
    // Тело функции
}

connect({
  host: 'localhost',
  port: 1829,
  user: 'peter'
});
```
В конце ставим по умолчанию пустой объект {} если функцию запустят без объекта

### Деструктуризация массивов

Достать числа из массива в переменные:
```javascript
const fib = [1, 1, 2, 4, 7, 11, 15, 19, 22];
const [a, b, c] = fib;

console.log(a, b, c);
```

Достать числа не по порядку, 2-е и 5-е. Пропуски отделяем запятыми:
```javascript
const [, a, , , b] = fib;

console.log(a, b); // 1, 7
```

Вывести данные из многомерного массива:
```javascript
const line = [ [10, 17], [14, 7] ];
const [ [p1x, p1y], [p2x, p2y] ] = line;

console.log(p1x, p1y, p2x, p2y);
```

Присвоить значение по умолчанию:
```javascript
const people = ['chris', 'sandra'];
const [a, b, c = 'guest'] = people;

console.log(a, b, c);
```

Рест-элементы:
```javascript
const people = ['chris', 'sandra', 'bob'];
const [a, ...others] = people;

console.log(others); // ['sandra', 'bob']
```

Разбиваем объект на массивы ключ-значение:
```javascript
const dict = {
  duck: 'quack',
  dog: 'wuff',
  mouse: 'squeak',
  hamster: 'squeak'
};

const res = Object.entries(dict);

console.log(res); // ["duck", "quack"], ["dog", "wuff"], ["mouse", "squeak"], ["hamster", "squeak"]
```

С помощью функции фильтра оставляем только тех, у кого значение (2 элемент) = squeak
```javascript
const res = Object.entries(dict)
  .filter((arr) => arr[1] === 'squeak');

console.log(res); // ["mouse", "squeak"], ["hamster", "squeak"]
```

То же самое, с деструктуризацией:
```javascript
const res = Object.entries(dict)
  .filter(([key, value]) => value === 'squeak');
```

Запишем в переменную res только названия животных, подходящих под условие:
```javascript
const res = Object.entries(dict)
  .filter(([key, value]) => value === 'squeak')
  .map(([key]) => key);
```

### Шаблонные строки

Можно вставлять переменные прямо в строки. Строки внутри символов бэктика(~)
```javascript
const user = 'Вася';
const num = 18;
const txt = 'Привет, ' + user + ' тебе пришло ' + num + ' сообщений';

const txt2 = `Привет ${user} тебе пришло ${num} сообщений`;

console.log(txt2); // Привет, Вася тебе пришло 18 сообщений
```

Внутри конструкции ${ } можно вставлять функции:
```javascript
const txt2 = `Сейчас ${Date.now()}`;

console.log(txt2); // Сейчас 1557659743354
```

Внутри бэктиков строка при переносе не прерывается:
```javascript
const html =
  '<ul>' +
  '<li>Пункт 1</li>' +
  '<li>Пункт 2</li>' +
  '</ul>';

const newHtml = `
  <ul>
    <li>Пункт 1</li>
    <li>Пункт 2</li>
  </ul>
`;
```

Можно комбинировать со значениями:
```javascript
const items = ['чай', 'кофе'];

const newHtml = `
  <ul>
    <li>${items[0]}</li>
    <li>${items[1]}</li>
  </ul>
`;
```

### Объекты

Значение в объекты стало записывать проще:
```javascript
const x = 1;
const y = 3;

const point = {
  x: x,
  y: y
};

const pointNew = {x, y};

console.log(pointNew); // {x: 1, y: 3}
```

Объединить/перезаписать свойства объектов:
```javascript
const defaults = {
  host: 'localhost',
  dbName: 'blog',
  user: 'admin'
};

const opts = {
  user: 'jonh',
  password: 'otopia'
};

const res = Object.assign({}, defaults, opts);

console.log(res); // {host: "localhost", dbName: "blog", user: "jonh", password: "otopia"}
```

Скопировать объект:
```javascript
const defaults = {
  host: 'localhost',
  dbName: 'blog',
  user: 'admin'
};

const schalloyCope = Object.assign({}, defaults); // Копия объекта defaults
```