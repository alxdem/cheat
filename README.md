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

### Spread оператор для объектов

Старый способ:

```javascript
const defaults = {
  host: 'localhost',
  dbName: 'blog',
  user: 'admin'
};

const opts = {
  user: 'john',
  password: 'utopia'
};

const res = Object.assign({}, defaults, opts); // Объект, куда записываем новые данные, объекты, которые записываем
```

Новый сопсоб:

```javascript
const result = {...defaults, ...opts};
```

Добавляем тут же новую переменную:

```javascript
const port = 8080;
const result = {...defaults, ...opts, port};
```

### Прототипы

Прототип - обычный объект.

Сначала проверяется объект, есть ли у него искомое свойство. Если оно есть - оно и используется. Если его нет - проверяется прототип этого объекта. Если в прототипе есть искомое свойство - используется оно.

Вместо: 

```javascript
const dog = {
  name: 'dog',
  voice: 'woof',
  say: function() {
    console.log(this.name, 'кричит', this.voice);
  }
};

const cat = {
  name: 'cat',
  voice: 'meow',
  say: function() {
    console.log(this.name, 'кричит', this.voice);
  }
};
```

Выносим одинаковую функцию в прототип:

```javascript
const animal = {
  say: function() {
    console.log(this.name, 'кричит', this.voice);
  }
};

const dog = {
  name: 'dog',
  voice: 'woof',
};
Object.setPrototypeOf(dog, animal); // animal - прототип объекта dog. Такой способ плохо скахывается на производительности

const cat = {
  name: 'cat',
  voice: 'meow',
};

dog.say();
```

Лучше делать так:
```javascript
const dog = Object.create(animal); // В скобках указываем прототип
dog.name = 'dog';
dog.voice = 'woof';
```

Конечный вариаент:

```javascript
const animal = {
  say: function() {
    console.log(this.name, 'кричит', this.voice);
  }
};

function createAnimal(name, voice) {
  const result = Object.create(animal);
  result.name = name;
  result.voice = voice;
  return result;
}

const dog = createAnimal('dog', 'woof');
```

Функция-конструктор называем с большой буквы:

```javascript
function Animal(name, voice) {
  this.name = name;
  this.voice = voice;
}

// Записываем в прототип Animal новый метод say
Animal.prototype.say = function() {
  console.log(this.name, 'кричит', this.voice);
};

const dog = new Animal('dog', 'woof');
```

Создать объект без прототипа:

```javascript
const obj = Object.create(null);
```

### Классы

Классы - синтаксический сахар. За основу берут прототип

```javascript
class Animal {

  constructor(name, voice) {
    this.name = name;
    this.voice = voice;
  }

  say() {
    console.log(this.name, 'кричит', this.voice);
  }

};

class Bird extends Animal { // extends говорит что классы Bird и Animal будут стоять в цепочке прототипов
 constructor(name, voice, canFly) {
    super(name, voice) // super - вызов конструктора супер-класса(родительского). Если мы наследуем класс, то обязательно вызываем супер-конструктор до того, как добавляем новое свойство
    this.canFly = canFly;
  }

  say() {
    console.log('Здесь переопределили метод конструктора Animal');
  }
}

const duck = new Bird('duck', 'quack');
// duck -> Bird.prototype -> Animal.prototype -> Object.prototype -> null
```

### Свойства классов


### Модули

Есть 2 функции в 1-м файле:

```javascript
function plus(a, b) {
  return a + b;
}

function minus(a, b) {
  return a - b;
}
```

Чтобы сделать их доступными в файле 2, экспортируем их:

```javascript
export {
  plus, minus
};
```

Во 2 файле получим их:

```javascript
import { plus, minus } from './file1';
```

Импортируемую функцию можно переименовать:

```javascript
import { plus as newFunc, minus } from './file1';

newFunc();
```

Экспортируемую функцию так же можно переименовать:

```javascript
export {
  plus as p, minus as m
};
```

Можно импортировать не отдельные функции, а все что есть:

```javascript
import * as calc from './file1';

calc.plus();
calc.minus();
```

Экспорт по умолчанию:

```javascript
export default Graph;
```

Тогда при импорте можно не ставить скобки:

```javascript
import Graph from './file1';
```

Можно сразу переименовать, без 'as':

```javascript
import NewFunc from './file1';
```

Можно не экспортировать конкретные функции, а получить сайд-эффект с другого файла (все функции в нем отработают):

```javascript
import './file1';
import './main.css';
```

Для импорта библиотеки используем только ее название (не путь):

```javascript
import joker from  'one-liner-joker';
```