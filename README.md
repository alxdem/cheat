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