__10 Советов по стилю кода:__
---
Самое главное: Код должен быть максимально читаемым и понятным для людей.


### 1.Фигурные скобки

лучше всего фигурные скобки всегда писать на той же строчке, на которой есть соответствующее ключевое слово – не на новой строке.

Например:

```
if (condition) {
  // делай это
  // ...и это
  // ...и потом это
}
```


А что если у нас однострочная запись?

Предлагаю писать так:


```
if (n < 0) {
  alert(`Степень ${n} не поддерживается`);

}
```
___
---
### 2.Отступы

Существует два типа отступов:

+ Горизонтальные отступы: два или четыре пробела или символа табуляции (клавиша Tab).

```
show(parameters,
     aligned, // 5 пробелов слева
     one,
     after,
     another
  ) {
  // ...
}
```

Одно из преимуществ пробелов над табуляцией заключается в том, что пробелы допускают более гибкие конфигурации отступов, чем символ табуляции.

+ Вертикальные отступы: пустые строки для разбивки кода на «логические блоки».

```
function pow(x, n) {
  let result = 1;
  //              <--
  for (let i = 0; i < n; i++) {
    result *= x;
  }
  //              <--
  return result;
}
```

Вставляйте дополнительный перевод строки туда, где это сделает код более читаемым. Не должно быть более 9 строк кода подряд без вертикального отступа.

___
---
### 3. Строгие проверки на равенство

Стремитесь к тому, чтобы вместо == использовать ===.

Если необдуманно использовать оператор == - это может серьёзно повлиять на программную логику. Это можно сравнить с тем, что некто ожидает, что пойдёт налево, но по какой-то причине вдруг идёт направо.


```
0 == false // true
0 === false // false
2 == "2" // true
2 === "2" // false

// пример
const value = "500";
if (value === 500) {
  console.log(value);
  // этот код не выполнится
}

if (value === "500") {
  console.log(value);
  // этот код выполнится
}
```
___
---
### 4. Переменные

Называйте переменные так, чтобы их имена раскрывали бы их сущность, их роль в программе. При таком подходе их удобно будет искать в коде, а тот, кто увидит этот код, легче сможет понять смысл выполняемых им действий. Если будет большой код, ты и сам можешь запутаться и потеряться в коде...


Плохо:
```
let daysSLV = 10;
let y = new Date().getFullYear();

let ok;
if (user.age > 30) {
  ok = true;
}
```
Хорошо:
```
const MAX_AGE = 30;
let daysSinceLastVisit = 10;
let currentYear = new Date().getFullYear();

...

const isUserOlderThanAllowed = user.age > MAX_AGE;

```

Также не стоит принуждать того, кто читает код, к тому, чтобы ему приходилось бы помнить о том, в каком окружении объявлена переменная.


Плохо:
```
const users = ["John", "Marco", "Peter"];
users.forEach(u => {
  doSomething();
  doSomethingElse();
  // ...
  // код
  // код
  // ...
  // Тут перед нами ситуация, в которой возникает WTF-вопрос "Для чего используется `u`?"
  register(u);
});
```
Хорошо:
```
const users = ["John", "Marco", "Peter"];
users.forEach(user => {
  doSomething();
  doSomethingElse();
  // ...
  // код
  // ...
  // ...
  register(user);
});
```
___
---
### 5.Создание переменной
 Всегда используйте ==let== или ==const== для объявления каждой переменной.


 Хорошо :
 ```
 let user = "-";
 const count = 2;
 ```
 Плохо:
 ```
var message = "Arror"; //устаревшее объявление переменной, может встречаться в коде до ES-2015, стоит избегать.
count = 2; //объявление переменной нет, ошибка.
 ```
А также ==не нужно задавать значение undefined==, оно присваивается автоматически.

___
---

### 6.Создание строки:

Используйте одинарные кавычки для строк



Плохo:
```
let name = "Боб Дилан"
```
Потому что двойные кавычки расходуют большее количество памяти

Хорошo:

``` 
let name = 'Боб Дилан'
```
___
---
### 7.Магические числа

Это жестко прописанные числа, значение которых не ясно. Впервые взглянув на этот код, вы даже не представляете, что это за число такое — 86400.


Плохо:
```
for(let i = 0; i < 86400; i += 1) {
  // ...
}
```
 
Хорошо:
```
const SECOND_IN_A_DAY = 86400;

for(let i = 0; i < SECOND_IN_A_DAY; i += 1) {
  // ...
}
```
Таким образом любой читатель будет знать, что речь идет о количестве секунд в сутках.
___
---
### 8.Глубокая вложенность
Если в вашем коде много вложенных циклов или условий, возможно, что-то следует вынести в отдельную функцию.
```
const exampleArray = [ [ [ 'value' ] ] ];

exampleArray.forEach((arr1) => {
  arr1.forEach((arr2) => {
    arr2.forEach((el) => {
      console.log(el)
    })
  })
})

// output: 'value'
```
В данном примере мы можем создать функцию, которая будет перебирать все массивы и возвращать итоговое значение:
```
const exampleArray = [ [ [ 'value' ] ] ];

const retrieveFinalValue = (element) => {
  if(Array.isArray(element)) {
    return fetchValue(element[0]);
  }

  return element;
}

// output: 'value'
```
___
---
### 9.Прекращайте писать комментарии

Хотя использование комментариев в коде может быть полезным, это также может служить признаком того, что ваш код не является самопоясняющим.

«Несмотря на то что комментарии по сути не являются ни чем-то плохим, ни чем-то хорошим, они часто используются в качестве «костылей». Код надо писать так, будто комментарии вообще не существуют. Это будет заставлять вас писать как можно более простым и самодокументирующим способом», – Джефф Атвуд.

==Код должен говорить сам за себя!==
___
---
### 10.О том, чего лучше не делать

Тому, кто хочет, чтобы его код был бы чистым, стоит стремиться к тому, чтобы не повторяться. Речь идёт о том, что нужно избегать ситуаций, в которых приходится писать один и тот же код. Кроме того, ==не нужно оставлять в кодовой базе неиспользуемые функции и фрагменты программ, которые никогда не выполняются.==

 ==Это — код, который присутствует в кодовой базе, но совершенно ничего не делает.== Такое случается, например, когда на определённом этапе разработки решают, что в некоем фрагменте программы больше нет смысла. Для того чтобы избавиться от подобных фрагментов кода, нужно внимательно просмотреть кодовую базу и убрать их. Легче всего избавляться от такого кода в тот момент, когда решено, что он больше не нужен. Позже можно забыть о том, для чего он использовался. Это значительно усложнит борьбу с ним.
