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

# React

## Создание проекта

Установка create-react-app на компьютер. Он даст нам правильно настроенный babel и Webpack со всеми нужными библиотеками:
```javascript
npm install -g create-react-app
```

Создание react-приложения (projectName - название новой папки с проектом):
```javascript
create-react-app projectName
```

### React элементы

Элемент - самый маленький "кирпичик".
Подключим React и ReactDom:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
```

Помещаем в константу html-код, JSX-код:
```javascript
const el = <h1>Hello World</h1>;
```

Если код состоит из нескольких строк (element-tree), то оборачиваем его в скобки:
```javascript
const el = (
  <div>
    <h1>My Todo List</h1>
    <input placeholder='search'/>
    <ul>
      <li>Learn React</li>
      <li>Build App</li>
    </ul>
  </div>
);
```

Рендерим его на странице. el - элемент, который выводим. 2-й параметр - место, куда выводим:
```javascript
ReactDOM.render(el, document.getElementById('root'));
```


### React компонент

Описание (Компонент начинается с большой буквы):
```javascript
const TodoList = () => {
  return (
    <ul>
      <li>Learn React</li>
      <li>Build App</li>
    </ul>
  );
};
```

Вывод:
```javascript
<TodoList/>
```

Рендерить можно в элемент, но не в компонент (не в App, а в <App/>):
```javascript
const App = () => {
  return(
    <div>
      <AppHeader/>
      <SearchPanel/>
      <TodoList/>
    </div>
  );
};

ReactDOM.render(<App/>, document.getElementById('root'));
```

### JSX

Корнем JSX-фрагмента должен быть только 1 элемент (внтури него могут быть другие теги):

Нельзя:
```javascript
const TodoList = () => {
  return (
    <span>text 1</span>
    <span>text 2</span>
  );
};
```

Можно:
```javascript
const TodoList = () => {
  return (
    <div>
      <span>text 1</span>
      <span>text 2</span>
    </div>
  );
};
```

Внутри JSX можно использовать переменные:
```javascript
const items = ['Petr', 'Ivan'];
const TodoList = () => {
  return (
    <div>
      <span>{ items[0] }</span>
      <span>{ items[1] }</span>
    </div>
  );
};
```

Можно использовать функции:
```javascript
<span>{ (new Date()).toString() }</span>
```

Теги:
```javascript
const loginBox = <span>Log in</span>

const TodoList = () => {
  return (
    <div>
      {loginBox} // Элемент
      <AppHeader/> // Компонент
      <SearchPanel/>
      <TodoList/>
    </div>
  );
};
```

Тернарный оператор:
```javascript
const isLogged = true;
const loginBox = <span>Log in</span>

const TodoList = () => {
  return (
    <div>
      {isLogged ? null : loginBox} // null - не выведет ничего
      <AppHeader/>
      <SearchPanel/>
      <TodoList/>
    </div>
  );
};
```

Внутри компонентов можно передавать объекты в свойствах:
```javascript
const App = () => {
  return(
    <div>
      <AppHeader/>
      <SearchPanel/>
      <TodoList items={['item1', 'item2']} />
    </div>
  );
};
```

2 атрибута, которые отличаются от html (className, htmlFor):
```javascript
return(
    <div>
      <TodoList className='class1' htmlFor='label' />;
    </div>
  );
```


### Структура React проекта

В папке src создаем папку для компонентов components. В ней создаем файлы js под каждый компонент.
В каждом компоненте подключаем React и экспортируем компонент:

```javascript
import React from "react";

const AppHeader = () => {
  return (
    <h1>My Todo List</h1>
  );
};

export default AppHeader;
```

В основной файл импортируем все компоненты. Указывать расширение файла не обязательно:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

import AppHeader from './components/app-header';
import SearchPanel from './components/search-panel';
import TodoList from './components/todo-list';
```


### Props - свойства компонента

Передается в каждую функцию. Внутри props будут названия всех свойст, которые вы передали объекту:
```javascript
const TodoListItem = (props) => {
  return <span>{ props.label }</span>;
};
```

Передаем свойства так (label):
```javascript
const TodoList = () => {
  return (
    <ul>
      <li><TodoListItem label='Leadr React' /></li>
      <li><TodoListItem label='Watch TV' /></li>
    </ul>
  );
};
```

То же самое, используя деструктуризацию:
```javascript
const TodoListItem = ( { label } ) => {
  return <span>{ label }</span>;
};
```

Если свойство передано без значение, оно по умолчанию true:
```javascript
<ul>
  <li><TodoListItem label='Leadr React' /></li>
  <li><TodoListItem
    label='Watch TV'
    important /></li>
</ul>
```

Если мы не передаем значение important, по-умолчанию будет false:
```javascript
const TodoListItem = ( { label, important = false } ) => {
  const style = {
    color: important ? 'tomato' : 'black'
  };

  return <span style={style}>{ label }</span>;
};
```

### Массисы как свойства компонентов

Получаем данные на самом верхнем уровне, в index.js:

```javascript
const App = () => {

  const todoDate = [
    { label: 'Drink coffee', important: false },
    { label: 'Watch TV', important: true },
    { label: 'Do React App', important: false },
  ];

  return(
    <div>
      <span>{ (new Date()).toString() }</span>
      <AppHeader/>
      <SearchPanel/>
      <TodoList todos={todoDate} />
    </div>
  );
};
```

Для каждого элемента создаем JSX-элемент. Выводим список элементов:

```javascript
const TodoList = ( {todos} ) => {

  const elements = todos.map((item => {
    return (
      <li>
        <TodoListItem
          label={item.label}
          important={item.important}/>
      </li>
    );
  }));

  return (
    <ul>
      { elements }
    </ul>
  );
};
```

Еще проще: взять каждое свойство из объекта item и передать его в качестве атрибута вместе со значение в TodoListItem:

```javascript
const TodoList = ( {todos} ) => {

  const elements = todos.map((item => {
    return (
      <li>
        <TodoListItem {...item}/>
      </li>
    );
  }));

  return (
    <ul>
      { elements }
    </ul>
  );
};
```

### Коллекции и ключи

Чтобы React не ругался на массив жлементов, нужно каждому элементу добавить уникльный ключ. Нужно для того, чтобы React обновлял только те элементы, которые изменились:

Добавим уникальный id каждому элементу. При получении данных с сервера, id уже будет передан:
```javascript
const todoDate = [
    { label: 'Drink coffee', important: false, id: 1 },
    { label: 'Watch TV', important: true, id: 2 },
    { label: 'Do React App', important: false, id: 3 },
  ];
```

Добавим id:
```javascript
return (
      <li key={item.id}>
        <TodoListItem {...item}/>
      </li>
    );
```

Индекс элементов в качестве key лучше не использовать - он не дает выигрыша в производительнсоти.

Чтобы не передавать лишнее свойство в компонент TodoListItem:
```javascript
<li key={item.id}>
  <TodoListItem {...item}/> // Лишнее свойство передается здесь
</li>
```

Воспользуемся деструктуризацией:

```javascript
<li key={item.id}>
  <TodoListItem {...item}/> // Лишнее свойство передается здесь
</li>
```

Делаем так:

```javascript
const elements = todos.map((item => {

    const { id, ...itemProps } = item; // Достаем id из объекта item. itemProps - все остальные свойста, которые не были деструктурированы в id

    return (
      <li key={id}>
        <TodoListItem {...itemProps }/>
      </li>
    );
  }));
```

### Импорт CSS

Быстрое подключение bootstrapcdn: https://www.bootstrapcdn.com/

Чтобы подключить css, нужное его импортировать в js-файл:
```javascript
import './todo-list.css';
```

Webpack собирает css и преобразует его.

### Структура проекта

Для каждого компонента - своя папка.

В index.js мы должны заменить пути на ./имя папки/имя компонента:
```javascript
import AppHeader from './components/app-header/app-header';
```

Чтобы не писать дважды имя пакпи и компонента, Webpack по умолчанию ищет внутри файл index.js.
Создадим внутри папки компонента файл index.js:
```javascript
import AppHeader from './app-header';

export default AppHeader;
```

Теперь в основном index.js можно импортировать компоненты так:
```javascript
import AppHeader from './components/app-header';
```

Если браузер выдает ошибку - перезапусти сборщик

Найти файл на 1 уровень выше (вместо одной точки ставим 2):
```javascript
import TodoListItem from '../todo-list-item';
```

### Компоненты-классы

Классы используются когда у компонента должно быть внутреннее состояние

У класса, в отличие от функции, пропсы хранятся в свойстве this.props.

```javascript
import React, { Component } from 'react';

import './todo-list-item.css';

export default class TodoListItem extends Component {

  render() {

    const { label, important = false } = this.props;

    const style = {
      color: important ? 'steelblue' : 'black',
      fontWeight: important ? 'bold' : 'normal'
    };

    return (
      <span className="todo-list-item">
      <span
        className="todo-list-item-label"
        style={style}>
        {label}
      </span>

      <button type="button"
              className="btn btn-outline-success btn-sm float-right">
        <i className="fa fa-exclamation" />
      </button>

      <button type="button"
              className="btn btn-outline-danger btn-sm float-right">
        <i className="fa fa-trash-o" />
      </button>
    </span>
    );
  }
}
```

Как решить что использовать: компонент-функцию или компонент-класс?
1. Если я сразу не вижу причин использовать класс - то пишем функцию;
2. Позже всегода можно изменить функцию на класс.

Тезисы:
1. Классы используются, когда нужно хранить сосотяние;
2. Классы наследуют React.Component;
3. props доступен через this.props.

### Обработка событий

Добавляем к элементу событие:

```javascript
<span
    className="todo-list-item-label"
    style={style}
    onClick={ ()=> console.log(`Done ${label}`) }>
    {label}
</span>
```

Контекст можно привязать с помощью bind:

```javascript
this.onLabelClick.bind(this)
```

Удобнее создать отдельную функцию и передать ее в onClick.
constructor нужен чтобы передать правильный this:

```javascript
  constructor() {
    super(); // Должны вызвать конструктор суперкласса (родителя), в данном случае это Component

    this.onLabelClick = () => {
      console.log(`Done: ${this.props.label}`);
    };
  }
  
  
    <span
        className="todo-list-item-label"
        style={style}
        onClick={ this.onLabelClick } >
        {label}
    </span>
```

### State - состояние компонета

Используем классы вместо функция для того, чтобы хранить внутреннее состояние компонетов. Функции этого не умеют

Установить state можно только один раз:
```javascript
this.state = {
  done: false
};
```

Менять его можно только с помощью функции setState:
```javascript
this.onLabelClick = () => {
  this.setState({
    done: true
  });
};
```

### Как работает setState

Чтобы изменить одно значение state, нужно его передать. При этом другие значение не перезапишутся:
```javascript
this.state = {
  done: false,
  important: false
};

this.onLabelClick = () => {
  this.setState({
    done: true
  });
};

this.onMarkImportant = () => {
  console.log('kkk');
  this.setState({
    important: true
  });
}
```


### Обновление состояния

Setstate принимает функцию;
Аргумент - текущий state

Чтобы переключать свойства в зависимости от состояния:
```javascript
    this.onMarkImportant = () => {
      console.log('kkk');
      this.setState((state) => {
        return {
          important: !state.important
        };
      });
    }
```

То же самое, но с деструктуризацией:
```javascript
    this.onMarkImportant = () => {
      console.log('kkk');
      this.setState(({important}) => {
        return {
          important: !important
        };
      });
    }
```

### Собственные события

Чтобы из одного элемента вызвать функцию в другом элементе:
Родительский элемент (функция onDeleted):
```javascript
  <li key={id} className="list-group-item">
    <TodoListItem
      {...itemProps }
      onDeleted={() => console.log('Deleted')}
    />
  </li>
```

Дочерний элемент (вызываем с помощью onClick):
```javascript
<button type="button"
        className="btn btn-outline-danger btn-sm float-right"
        onClick={this.props.onDeleted}
        >
  <i className="fa fa-trash-o" />
</button>
```

### Удаление элемента

Перепишем App из функции в класс, чтобы можно было менять состояние.

```javascript
state = {
    todoData: [
      { label: 'Drink Coffee', important: false, id: 1 },
      { label: 'Make Awesome App', important: true, id: 2 },
      { label: 'Have a lunch', important: false, id: 3 }
    ]
  };

  deleteItem = (id) => {
    this.setState(({ todoData }) => {

      const idx = todoData.findIndex((el) => el.id === id);
      const before = todoData.slice(0, idx);
      const after = todoData.slice(idx + 1);
      const newArray = [...before, ...after];

      return {
        todoData: newArray
      };

    });
  };
```

### Добавление элемента

Добавить элемент в конец массива:
```javascript
const newArr = [ ...oldArray, newItem];
```

В конец:
```javascript
const newArr = [ newItem, ...oldArray];
```

### Данные

Если данные нужно использовать в нескольких компонентах - хранить их нужно в родительском компоненте
Чтобы поднять данные вверх по иерархии используем события

### SetState - обновление элементов

Обновляем список данных. 
1. Копируем изменяемый объект (Меняем в нем нужное свойство done: !oldItem.done)
2. Копируем массив с объектами
3. Удаляем из нового массива элемент по id
4. На его место вставляем новый элемент
5. Отправляем новые данные в state

```javascript
onToggleDone = (id) => {
    this.setState(({ todoData }) => {
        const idx = todoData.findIndex((el) => el.id === id);
        
        const oldItem = todoData[idx];
        const newItem = {...oldItem, done: !oldItem.done};
        
        const newArray = [
            ...todoData.slice(0, idx),
            newItem,
            ...todoData.slice(idx + 1)
        ];
        
        return {
          todoData: newArray
        };
    });
};
```

### Работа с формами

### Контролируемые компоненты

### Поиск

Добавляем в state новый элемент term, в нем храним текст для поиска
```javascript
state = {
    todoData: [
      this.createTodoItem('Drink Coffee'),
      this.createTodoItem('Make Awesome App'),
      this.createTodoItem('Have a lunch'),
    ],
    term: ''
  };
```

Отфильтруем все элементы, которые содержать строку term:
```javascript
search = (items, term) => {

    if(term.length === 0) {
      return items;
    }

    return items.filter((item) => {
      return item.label.indexOf(term) > -1;
    });
  };
```

Меняем term при вводе текста:
```javascript
onSearchChange = (term) => {
    this.setState( {term} );
  };
```

## Работа с сервером

### Выбор HTTP API

XMLHTTPRequest - устарел
Fetch API - современный

Библиотеки для работы с API:
1. Axios
2. Superagent
3. Got
4. Request
5. Reqwest


### Как работает Fetch API

Делаем запрос к серверу и получаем ответ сервера. Сначала получаем результат, а затем вытаскиваем тело из результата:
```javascript
fetch('https://swapi.co/api/people/1/')
  .then((response) => {
    return response.json();
  })
  .then((body) => {
    console.log(body);
  })
```

Тот же код, через функцию:
```javascript
const getResource = async (url) => {
  const res = await fetch(url) // await - ждем результат промиса
  const body = await res.json();
  return body;
};

getResource('https://swapi.co/api/people/1/')
  .then((body) => {
    console.log(body);
  });

fetch('https://swapi.co/api/people/1/')
  .then((response) => {
    return response.json();
  })
  .then((body) => {
    console.log(body);
  })
```

### Обработка ошибок в Fetch API

Получаем ошибку, если нет соединения или лег сервер:
```javascript
// Если нет интернета, лег сервер
  .catch((err) => {
    console.error(err);
  });
```

Если вервер прислал ответ, но с ошибкой:
```javascript
const getResource = async (url) => {
  const res = await fetch(url) // await - ждем результат промиса
  
  // Здесь обрабатываем ошибку. res.ok - булево значение. res.status - код ошибки
  if(!res.ok) {
    throw new Error(`Could not Fetch ${url}, received ${res.status}`);
  }

  const body = await res.json();
  return body;
};
```

### Создаем клиент для  API

1. Создаем для этого класс, чтобы изолировать от остального кода.
2. Компоненты не должны знать откуда берется код.

```javascript
class SwapiService {

  _apiBase = 'https://swapi.co/api'; // Начинаем с нижнего подчеркивания, т. к. это приватная часть и ее не следует изменять внешне

  async getResource(url) {
    const res = await fetch(`${this._apiBase}${url}`); // await - ждем результат промиса

    if(!res.ok) {
      throw new Error(`Could not Fetch ${url}` +
        `received ${res.status}`);
    }

    return await res.json();
  }

  async getAllPeople() {
    const res = await this.getResource(`/people/`);
    return res.results;
  }

  getPerson(id) {
    return this.getResource(`/people/${id}/`);
  }
}

const swapi = new SwapiService();

swapi.getPerson(4).then((people) => {
  console.log(people.name);
});
```

### Компонент, который получает данные с сервера

Чтобы обновить данные в компоненте, вызываем нужную функцию в конструкторе когда он создается:

```javascript
constructor() {
    super();
    this.updatePlanet();
  }
```

### Трансформация данных API



## Жизненный цикл компонентов

1. Mounting (вызывает эту функцию, когда компонент создается и отображается первый раз)
constructor() => render() => componentDidMount()
2. Updates (компонент уже отобразился и работает и может получать обновления. Вызывается, когда компонент обновился или получил новое свойство)
New props / setState() = > render() => componentDidUpdate()
3. Unmounting (вызывается перед удалением компонента)
componentWillUnmount()
4. Error (когда компонент получает ошибку)
componentDidCatch()

### componentDidMount
Вызывать функции в componentDidMount, а не конструкторе

### componentDidUpdate
Вызывается, когда компонент обновился или получил новое свойство

Принимает 2 свойства:
componentDidUpdate(prevProps, prevState)
prevProps - предыдущая версия пропс
prevState - предыдущая версия state

```javascript
componentDidUpdate(prevProps) {
    // Важно! Проверять текущий и прошлый state, иначе это будет бесконечный цикл: каждая смена state вызывает CDU, а он - смену state
    if(this.props.personId !== prevProps.personId) {
      this.updatePerson();
    }
  }
```

### componentWillUnmount
Вызывается перед тем, как удалить компонент.
Здесь удаляем таймеры, интервалы, запросы к серверу, привязанные сокеты.
В момент вызова DOM еще находится на странице.

### componentDidCatch
Задача метода - обработка непойманных ошибок в других циклах ниже по иерархии.
Нужен чтобы сообщить пользователю об ошибке.
Добавляем в компонент верхнего уровня app.js.

Если мы не хотим чтобы при ошибке в одном компоненте рушилась вся страница, добавляем componentDidCatch на уровень каждого независимого компонента. Ошибка будет подыматься выше до дом дереву до тех пор, пока ее не перехватит componentDidCatch в компоненте.

Компоненты, которые перехватывают ошибки - границы ошибок.

componentDidCatch(error, info):
error - ошибка, которая привела к вызову метода
info - детали в каком компоненте произошла ошибка

## Паттерны

### Функции

Можно передавать функцию в компонент. Здесь получаем список элементов по API:
```javascript
 <ItemList
  onItemSelected={this.onPersonSelected}
  getData={this.swapiService.getAllStarships}
/>
```

### Render-функции
Рендер-функция - функция, которую передаем в компонент. Она занимается рендерингом части или всего компонента.

Передаем в компонент функцию renderItem:
```javascript
<ItemList
  onItemSelected={this.onPersonSelected}
  renderItem={({name, model}) => `${name} (${model})`}
/>
```

Внутри функции помещаем результат выполнения функции в label и просто выводим его:
```javascript
const label = this.props.renderItem(item);
return (
<li className="list-group-item">
  {label}
</li>
);
```
Функция может возвращать как строку, так и React-компонент.
```javascript
renderItem={({name}) => (<span>{name} <button>!</button></span>)}
```

### Свойства-элементы
Элемент-контейнер принимает разные данные и оборвчивает их в заданную структуру.

Элемент-контейнер:
```javascript
const Row = ({left, right}) => {
  return (
    <div className="row mb2">
      <div className="col-md-6">
        {left}
      </div>
      <div className="col-md-6">
        {right}
      </div>
    </div>
  );
};
```

Передаем в него данные:
```javascript
render() {
  const itemList = (
    <ItemList
      onItemSelected={this.onPersonSelected}
      getData={this.swapiService.getAllPeople}
      renderItem={({name, gender, birthYear}) => `${name} (${gender}, ${birthYear})`}
    />
    );
    
    const personDetails = (
    <PersonDetails personId={this.state.selectedPerson} />
    );
      
  return (
    <Row left={itemList} right={personDetails}/>
  );
}
```

### Children

this.props.children; - можно передать любой тип данных: строка, объект, функция.
```javascript
<ItemList>
  {[1, 2, 4]}
</ItemList>
```

## Routing

На самом деле мы не переходим с одной страницы на другую, мы остаемся все время на одной странице, мы просто говорим какие компоненты скрыть и какие показать

### Установка
yarn add react-router-dom

React Router - не часть React, есть и другие библиотеки.

Подключаем:
```javascript
import { BrowserRouter, Route} from 'react-router-dom';
```

Оборачиваем все что мы будем выводить в компонент Router:
```javascript
return (
      <Router>
        <div className="stardb-app">
          <Header />
          ...
      </Router>
```

Внутри компонент Route. У него 2 свойства:
1. path - путь;
2. component - какой компонент отображать при выбранном пути (По сути, компонент - наша страница, на которой мы выводим другие компоненты).

```javascript
<Router>
  <Route path="/main" component={MainPage}/>
  <Route path="/people" component={PeoplePage}/>
</Router>
```

### Link (Переключение страниц)

Импортируем Link в том компоненте, где мы будем выводить ссылки на страницы
```javascript
import { Link } from 'react-router-dom';
```

Заменяем теги <a> на компонент <Link>:
```html
<a href="#/people">People</a> // Было
<Link to='/people'>People</Link>  // Стало
```

Используем компонент <Link> вместо обычной ссылки для того, чтобы при переходе на другую страницу не ререзагружать страницу

### Как работает Route
В компонент Route можно передевать рендер-функцию:

```html
<Route path="/" render={() => <h2>Welcome to StarDB</h2> }/>
```
Когда React Router проверяет какие компоненты нужно отобразить, он отвечает на вопрос, содержит ли текущий адрес тот путь, который указан в path. Например, путь '/' содержится в '/people'.

Чтобы проверять точный адрес, передадим пропс exact={true}:
```javascript
<Route
    path="/"
    render={() => <h2>Welcome to StarDB</h2> }
    exact={true}
/>
```

Для одной страницы можно выводить разные комбинации:
```javascript
<Route
    path="/people"
    render={() => <h2>People</h2> }
/>
<Route path="/people" component={PeoplePage}/>
```

### Динамические пути

В Route можно добавлять динамический параметр (:id):
```javascript
<Route path="/starships/:id" component={PeoplePage}/>
```

В рендер-функцию React-Router передаст 3 параметра: 
match - как именно совпал path с адресом в браузере. Здесь есть параметр id
location - текущее положение роутера на текущей странице
history - внутренний api Router, для перехода внутри страниц

```javascript
<Route path="/starships/:id"
     render={({match, location, history}) => {
       return <StarshipDetails/>
     }}
/>
```

### withRouter
В Реакте лучше использовать компоненты-функции, нежели компоненты-классы. Как только компоненту не нужен state - переделываем его в функцию.

Когда мы используем Route вместе с компонентом, роутер не будет автоматически добавлять в свойства этого компонента объекты match, location, history.
Чтобы получить доступ к ним, нужно использовать компонент высшего порядка, который использует контекст внутри себя.  

Импортируем withRouter (компонент высшего порядка):
```javascript
import { withRouter } from 'react-router-dom';
```

Возвращаем withRouter и передаем в него наш компонент:
```javascript
export default StarshipsPage; // Было
export default withRouter(StarshipsPage); // Стало
```

Теперь withRouter передаст в StarshipsPage объекты match, location, history.

Нам нужен объект history.
Чтобы переключить страницу нужно вызвать метод history.push():
```javascript
const StarshipsPage = ({ history }) => {
    return (
      <StarshipList
        onItemSelected={(itemId) => {
          history.push(`/starships/${itemId}`);
        }}
      />
    );
};

export default withRouter(StarshipsPage);
```

### Относительные пути
Как формируются пути в вебе:
```javascript
/starships/ + 5 = /starships/5
/starships  + 5 = /5
```
В первом случае путь указывает на папку(/), а во втором на файл

Заменим ссылки:
```javascript
<Link to="/people">People</Link> // Было
<Link to="/people/">People</Link> // Стало
```

Адреса промежуточных страниц должны заканчиваться на слэш /

### Опциональные параметры

Если в конце добавить '?', то параметр будет считаться опциональным:
```javascript
<Route path="/people/:id" component={PeoplePage} /> // Так, при переходе по адресу /people/ PeoplePage не отобразиться
<Route path="/people/:id?" component={PeoplePage} /> // А так, отобразится
```

### Авторизация и закрытые страницы
Redirect - компонент React для перенаправления на другую страницу:
```javascript
import { Redirect } from 'react-router-dom';

return <Redirect to={'/login'} />;
```

### Обработка несуществующих адресов (Switch)

Импортируем switch:
```javascript
import { BrowserRouter as Router, Route, Redirect, Switch } from 'react-router-dom';
```

Обернем в Switch все существующие роуты:
```javascript
<Switch>
    <Route path="/"
           render={() => <h2>Welcome to StarDB</h2>}
           exact />
    <Route path="/people/:id?" component={PeoplePage} />
    <Redirect to='/'/>
</Switch>
```
Когда Switch пройдет все варианты и ни на одном не остановится, выполним Redirect.
Внутри Switch будет отрисован максимум один элемент.

Вместо редиректа можно выводить сообщение на той же странице. Rout без path будет срабатывать всегда.
```javascript
<Route render={() => <h2>Page not found</h2>} />
```

## Redux

### Введение

Property Drill (State в самом верхнем компоненте) - проблема в React, когда для того, чтобы обновить состояние из самого нижнего компонента, нужно было поднимать событие в самый верхний по цепочке компонентов.

Fragmented state (State у кахдого компонента) - сложно масштабировать, легко поломать.

Делаем одни глобальный стейт, и, в то же время, делаем его доступным для каждого компонента независимо от уровня вложенности
Запретим компонентам писать в глобальный стейт, только читать.
Всю логику по обновлению Стейта вынесем в отдельную функцию-reducer.
Компоненты могут создавать события (Actions), которые передаем в функцию-reducer.

<img src="/pics/1.jpg" alt="Работа Redux" />

### Установка библиотек

С Redux можно работать без React.
Устанавливаем:
```javascript
npx create-react-app my-app // npx
npm init react-app my-app // npm
yarn create react-app project-name // yarn
```

Установим 2 библиотеки: 
1. redux
2. react-redux - библиотека, упрощает работу с React
```javascript
yarn add redux react-redux
```

### Reducer
Reducer - функция, которая получает 2 значения: текущий state и действие, которую нужно совершить (action)
1. Если изначального state нет, то присвоим ему 0
2. action - объект. Главное, чтобы у него всегда было поле type.

Принцип reducer на чистом JS:
```javascript
const reducer = (state = 0, action) => {

  switch (action.type) {
    case 'INC':
      return state + 1;

    // Если не распознали action.type, возвратим прежний state
    default:
      return state;
  }
};

let state = reducer(undefined, {});

state = reducer(state, { type: 'INC' });
console.log(state); // 1
state = reducer(state, { type: 'INC' });
console.log(state); // 2
```

### Redux Store
Redux Store - это Reducer и State

```javascript
import { createStore } from 'redux';

const reducer = (state = 0, action) => {

  switch (action.type) {
    case 'INC':
      return state + 1;

    // Если не распознали action.type, возвратим прежний state
    default:
      return state;
  }
};

const store = createStore(reducer);

// Вызываю функцию, когда store изменился
store.subscribe(() => {
  console.log(store.getState());
});

store.dispatch({type: 'INC'}); // Выполним действие
store.dispatch({type: 'INC'});
```

Создаем Store:
```javascript
const store = createStore(reducer);
```

Получить текущий state:
```javascript
store.getState();
```

Вызвать действие:
```javascript
store.dispatch({type: 'INC'});
```

Подписаться на обновления store:
```javascript
store.subscribe(() => {
  console.log(store.getState());
});
```

### Чистые функции
- Возвращаемое значение зависит только от аргументов
- У функции нет побочных эффектов (она должна только возвразать результат. Не записывать в базу, не менять кэш, стейт, не влиять на другую функцию, менять DOM, вызывать сервер)

Функция-редьюсер - должна быть чистой функцией!

### UI для Redux
```javascript
import { createStore } from 'redux';

const reducer = (state = 0, action) => {
  switch (action.type) {
    case 'INC':
      return state + 1;

    case 'DEC':
      return state - 1;

    default:
      return state;
  }
};

const store = createStore(reducer);

document.getElementById('inc').addEventListener('click', () => {
    store.dispatch({type: 'INC'});
  });
document.getElementById('dec').addEventListener('click', () => {
    store.dispatch({type: 'DEC'}); // Обновдяем состояние
  });

const update = () => {
  document.getElementById('counter').innerHTML = store.getState();
};

store.subscribe(update); // Подписываем функцию update на изменения стейта
```

### Действия с параметрами
Для передачи параметра:
```javascript
const reducer = (state = 0, action) => {

  switch (action.type) {
    case 'RND':
      return state + action.payload; // Добавляем на параметр

    // Если не распознали action.type, возвратим прежний state
    default:
      return state;
  }
};

const store = createStore(reducer);

document.getElementById('rnd').addEventListener('click', () => {
    const payload = Math.floor(Math.random() * 10);
    store.dispatch({
      type: 'RND',
      payload // Передаем доп. параметры
    });
  });

const update = () => {
  document.getElementById('counter').innerHTML = store.getState();
};

store.subscribe(update); // Подписываем функцию update на изменения стейта
```

### Action Creator
Action Creator - функции-создатели действий. Нужны для удобства.

Вместо того, чтобы каждый раз передавать объект с длинным названием (в котором можно сделать опечатку), можно поместить объект в функцию конструктор:
```javascript
const inc = () => ({ type: 'INC' });

document
  .getElementById('inc')
  .addEventListener('click', () => {
    store.dispatch(inc()); // Здесь возвращаем наш объект
  });
```

### Структура проекта
- Выносим action creator в отдельный файл
- Выносим Reducer в отдельный файл

В файл actions.js:
```javascript
export const inc = () => ({ type: 'INC' });
export const dec = () => ({ type: 'DEC' });
export const rnd = (payload) => ({ type: 'RND', payload });
```

Импортируем их:
```javascript
import reducer from './reducer';
import { inc, dec, rnd } from './actions';
```

### bindActionCreators()

bindActionCreators() - связывает функцию action creator с функцией dispatch()
```javascript
const { inc, dec, rnd } =bindActionCreators(actions);
```

Сократим код, деструктурировав dispatch:
```javascript
const store = createStore(reducer);
const { dispatch } = store;

document
  .getElementById('inc')
  .addEventListener('click', () => {
    store.dispatch(inc()); // Было
    dispatch(inc()); // Стало
  });
```

Можно вынести 2 функции в одну:
```javascript
const store = createStore(reducer);
const { dispatch } = store;
const incDispatch = () => dispatch(inc());
const rndDispatch = (payload) => dispatch(rnd(payload));

document
  .getElementById('inc')
  .addEventListener('click', incDispatch);

document
  .getElementById('rnd')
  .addEventListener('click', () => {
    const payload = Math.floor(Math.random() * 10);
    rndDispatch(payload);
  });
```

Эта функция будет возвращать новую функцию

Импортируем bindActionCreators:
```javascript
import { createStore, bindActionCreators } from 'redux';
```