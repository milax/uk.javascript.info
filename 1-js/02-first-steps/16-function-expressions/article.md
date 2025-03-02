# Функціональні вирази

Для JavaScript функція — це не "магічна мовна структура", a значення особливого ґатунку.

Синтаксис, що ми раніше використовували, називається [*Оголошення Функції*](https://developer.mozilla.org/uk/docs/Web/JavaScript/Guide/Functions#Оголошення_функції) (Function Declaration):

```js
function sayHi() {
  alert( "Привіт" );
}
```

Існує й інший синтаксис для створення функції, що називають [*Функціональним Виразом*](https://developer.mozilla.org/uk/docs/Web/JavaScript/Guide/Functions#Функціональні_вирази) (Function Expression).

Він дозволяє створювати функцію всередині будь-якого виразу.

Наприклад:

```js
let sayHi = function() {
  alert( "Привіт" );
};
```

Ми бачимо змінну `sayHi`. В якості значення, їй присвоюється нова функція `function() { alert("Привіт"); }`.

Оскільки створення функції відбувається в контексті присвоєння виразу (праворуч від `=`), це називається *Функціональним виразом* (Function Expression).

Зауважте, після ключового слова `function` немає назви функції. Для функціональних виразів це допустимо.

В цьому прикладі, ми одразу присвоюємо функцію цій змінній, тож, інакше кажучи, код в прикладі "створює функцію і кладе її в змінну `sayHi`".

В складніших ситуаціях, з якими ми стикнемося пізніше, функцію можна створити і відразу ж викликати або запланувати її на відтерміноване виконання. Такі функції ніде не зберігаються, тому вони залишаються анонімними.

## Функція -- це значення

Повторімо: немає значення, яким чином створено функцію, функція -- це завжди значення. В обох прикладах вище, функція зберігається в змінній `sayHi`.

Ми навіть можемо вивести це значення, використовуючи `alert`:

```js run
function sayHi() {
  alert( "Привіт" );
}

*!*
alert( sayHi ); // показує код функції
*/!*
```

Зауважте, що останній рядок не викликає функцію тому, що після `sayHi` немає дужок. Існують мови програмування, в яких будь-яке звертання до імені функції спричиняє її виконання, але JavaScript — не одна з них.

У JavaScript функція — це значення, тому ми можемо поводитись з нею, як і з іншими значеннями. Код вище показує її рядкове представлення — вихідний код.

Звичайно, функція — особливе значення, у тому сенсі, що ми можемо здійснити її виклик за допомогою дужок: `sayHi()`.

Але це все-таки значення. Тому ми можемо працювати з нею, як і з іншими значеннями.

Скажімо, ми можемо скопіювати функцію в іншу змінну:

```js run no-beautify
function sayHi() {   // (1) створюємо
  alert( "Привіт" );
}

let func = sayHi;    // (2) копіюємо

func(); // Привіт     // (3) викликаємо копію (працює!)
sayHi(); // Привіт    //     ось так теж спрацює (а чому ні?)
```

Розглянемо детально, що тут відбулось:

1. Оголошення Функції `(1)` створює саму функцію і кладе її значення у змінну `sayHi`.
2. Рядок `(2)` копіює це значення в змінну `func`. Ще раз зауважте: після `sayHi` немає дужок. Якби вони там були, тоді `func = sayHi()` записав би *результат виклику* `sayHi()` у `func`,а не *саму функцію* `sayHi`.
3. Тепер ми можемо викликати функцію двома шляхами: `sayHi()` або `func()`.

Також ми могли використати Функціональний Вираз у першому рядку, щоб визначити `sayHi`:

```js
let sayHi = function() { // (1) створити
  alert( "Привіт" );
};

let func = sayHi;
// ...
```

Результат був би таким самим.


````smart header="Для чого потрібна крапка з комою в кінці?"
Ви можете запитати, чому Функціональний Вираз містить крапку з комою `;` в кінці, а Оголошення Функції — ні:

```js
function sayHi() {
  // ...
}

let sayHi = function() {
  // ...
}*!*;*/!*
```

Відповідь проста: тут Функціональний Вираз (у вигляді `function(…) {…}`) створений всередині інструкції присвоєння: `let sayHi = …;`. Крапка з комою `;` не є частиною синтаксису функції. Вона є частиною інструкції, тому її рекомендується ставити в кінці інструкції.

Крапку з комою доцільно було б поставити і для простішої інструкції, наприклад для `let sayHi = 5;`.
````

## Колбеки (функції зворотного виклику)

Розглянемо інші приклади передачі функції як значення та використання Функціональних Виразів.

Для цього напишемо функцію `ask(question, yes, no)` з трьома параметрами:

`question`
: Текст запитання

`yes`
: Функція, що буде викликатись, якщо відповідь "Так"

`no`
: Функція, що буде викликатись, якщо відповідь "Ні"

Функція повинна поставити запитання `question` і, залежно від відповіді користувача, викликати `yes()` або `no()`:

```js run
*!*
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}
*/!*

function showOk() {
  alert( "Ви погодились." );
}

function showCancel() {
  alert( "Ви скасували виконання." );
}

// використання: функції showOk, showCancel передаються як аргументи для ask
ask("Ви згодні?", showOk, showCancel);
```

Такі функції є досить практичними. Головна відмінність між функцією `ask` у реальних програмах та прикладі вище, полягає в тому, що перша може використовувати більш складні способи взаємодії з користувачем, ніж звичайний `confirm`. У браузерах така функція зазвичай показує гарненьке модальне вікно з запитанням. Але це вже інша історія.

**Аргументи `showOk` та `showCancel` функції `ask` називаються *функціями зворотного виклику* або просто *колбеками*.**

Суть полягає в тому, що ми передаємо функцію і очікуємо, що вона буде викликана (англ. "called back") пізніше, якщо це буде потрібно. У нашому випадку, `showOk` стає колбеком, якщо відповідь — "yes", а `showCancel`, якщо відповідь — "no".

Ми можемо використати Функціональний Вираз, щоб записати туж саму функцію коротше:

```js run no-beautify
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}

*!*
ask(
  "Ви згодні?",
  function() { alert("Ви погодились."); },
  function() { alert("Ви скасували виконання."); }
);
*/!*
```

У цьому прикладі функції оголошені всередині виклику `ask(...)`. Вони не мають власного ім'я, тому називаються *анонімними*. До таких функцій не можна доступитись поза `ask` (бо вони не присвоєні змінним), але це саме те, що нам потрібно.

Подібний код, що з'явився в нашому скрипті є природним, в дусі JavaScript.

```smart header="Функція — це значення, що відображає \"дію\""
Звичайні значення, як-от: рядки або числа, відображають *дані*

Функцію, з іншого боку, можна сприймати як *дію*.

Ми можемо передавати її від змінної до змінної і викликати коли заманеться.
```


## Функціональний Вираз проти Оголошення Функції

Сформулюймо ключові відмінності між Оголошенням Функції та Функціональним Виразом.

По-перше, синтаксис: як розрізняти їх в коді.

- *Оголошення Функції:* функція оголошується окремою конструкцією "function..." в основному потоці коду.

    ```js
    // Оголошення Функції
    function sum(a, b) {
      return a + b;
    }
    ```
- *Функціональний Вираз:* функція створюється як частина іншого виразу чи синтаксичної конструкції. Нижче, створення функції відбувається в правій частині "виразу присвоєння" `=`:

    ```js
    // Функціональний Вираз
    let sum = function(a, b) {
      return a + b;
    };
    ```

Більш тонка відмінність в тому, *коли* функція буде створена рушієм Javascript.

**Функціональний Вираз буде створено тільки тоді, коли до нього дійде виконання і тільки після цього він може бути використаний.**

Щойно потік виконання досягне правої частини у присвоєнні `let sum = function…`, функцію буде створено і з цього моменту її можна буде використати (присвоїти змінній, викликати тощо ).

У випадку з Оголошенням Функції все інакше.

**Синтаксис Оголошення Функції дозволяє викликати функцію раніше, ніж вона були визначена в коді**

Наприклад, глобальне Оголошення Функції буде доступним з будь-якого місця в скрипті.

Така поведінка спричинена особливостями внутрішніх алгоритмів. Коли JavaScript готується до виконання скрипта, він спочатку шукає всі глобальні Оголошення Функцій і на їх основі створює функції. Цей процес можна вважати "фазою ініціалізації".

Після того, як всі Оголошення Функцій були оброблені, рушій починає виконання коду.

Це, наприклад, буде працювати:

```js run refresh untrusted
*!*
sayHi("Іван"); // Привіт, Іван
*/!*

function sayHi(name) {
  alert( `Привіт, ${name}` );
}
```

Функцію `sayHi` було створено, коли JavaScript готувався до виконання скрипта і вона буде доступною з будь-якого місця.

...З Функціональним Виразом це не спрацювало б:

```js run refresh untrusted
*!*
sayHi("Іван"); // помилка!
*/!*

let sayHi = function(name) {  // (*) більше ніякої магії
  alert( `Привіт, ${name}` );
};
```

Створення функцій, визначених Функціональними Виразами, відбувається тоді, коли до них доходить потік виконання. Це станеться тільки при досягненні рядку з зірочкою `(*)`. Занадто пізно.

Ще однією особливістю Оголошення Функції є її блокова область видимості.

**У суворому режимі, якщо Оголошення Функції знаходиться в блоці `{...}`, то функція доступна усюди всередині блоку. Але не зовні.**

Уявімо, що нам потрібно визначити функцію `welcome()` залежно від змінної `age`, яку ми отримаємо під час виконання коду. Далі в скрипті нам буде потрібно викликати цю функцію.

Якщо ми використаємо Оголошення Функції, то це не буде працювати:

```js run
let age = prompt("Скільки вам років?", 18);

// оголошуємо функцію відповідно до умови
if (age < 18) {

  function welcome() {
    alert("Привіт!");
  }

} else {

  function welcome() {
    alert("Вітання!");
  }

}

// ...спробуємо викликати функцію
*!*
welcome(); // помилка в суворому режимі (ReferenceError: welcome is not defined)
*/!*
```

Це тому, що Оголошення Функції доступне тільки всередині блоку, що його містить.

Інший приклад:

```js run
let age = 16; // для прикладу присвоїмо 16

if (age < 18) {
*!*
  welcome();               // \   (виконується)
*/!*
                           //  |
  function welcome() {     //  |  
    alert("Привіт!");      //  |  Оголошення Функції доступне
  }                        //  |  усюди в блоці, що його містить
                           //  |
*!*
  welcome();               // /   (виконується)
*/!*

} else {

  function welcome() {
    alert("Вітання!");
  }
}

// Тут фігурні дужки закриваються,
// тому Оголошення Функції всередині них нам не доступне

*!*
welcome(); // помилка в суворому режимі (ReferenceError: welcome is not defined)
*/!*
```

Що можна зробити, щоб функція `welcome` стала видимою поза `if`?

Правильніше було б використати Функціональний Вираз і присвоїти `welcome` змінній, що оголошена поза блоком `if` і доступна для нас.

Цей код працює як нам і потрібно:

```js run
let age = prompt("Скільки вам років?", 18);

let welcome;

if (age < 18) {

  welcome = function() {
    alert("Привіт!");
  };

} else {

  welcome = function() {
    alert("Вітання!");
  };

}

*!*
welcome(); // тепер все гаразд
*/!*
```

Можна спростити цей код, використавши умовний оператор `?`:

```js run
let age = prompt("Скільки вам років?", 18);

let welcome = (age < 18) ?
  function() { alert("Привіт!"); } :
  function() { alert("Вітання!"); };

*!*
welcome(); // спрацює
*/!*
```


```smart header="Коли використовувати Оголошення Функції, а коли -- Функціональний Вираз?"
Зазвичай, коли нам потрібна функція, то найперше потрібно розглянути синтаксис Оголошення Функції. Він дає нам більше свободи у тому, як організовувати код, оскільки дозволяє викликати функції ще до їх визначення.

Також функції `function f(…) {…}` простіше помітити в коді, ніж `let f = function(…) {…};`. Оголошення Функції легше "ловляться очима".

...Але якщо з якоїсь причини Оголошення Функції нам не підходить або нам потрібно визначити функцію згідно умови (як це було в прикладі), то слід використати Функціональний Вираз.
```

## Підсумки

- Функції — це значення. Ми можемо присвоїти їх змінній, копіювати чи визначити у будь-якій частині коду.
- Якщо функція визначена як окрема інструкція в основному потоці коду, то це називають "Оголошенням Функції".
- Якщо функція була створена як частина іншого виразу, тоді це — "Функціональний Вираз".
- Обробка Оголошень Функцій виконується перед виконанням блоку коду. Такі функції є видимими у всьому блоці.
- Функції, що були визначені за допомогою Функціонального Виразу, будуть створені тільки тоді, коли до них дійде потік виконання.

Зазвичай, коли нам потрібно визначити функцію, то краще віддати перевагу синтаксису Оголошення Функції, бо така функція є видимою навіть перед її визначенням в коді. Це дає нам більшу гнучкість в організації коду та й сам синтаксис є більш читабельним.

Таким чином, використовувати Функціональний Вираз потрібно тільки тоді, коли Оголошенням Функції не підходить для вирішення нашої задачі. Ми розглянули декілька таких прикладів у цьому розділі і побачимо ще в майбутньому.
