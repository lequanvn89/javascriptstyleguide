# JavaScript (ES6) style guide
Пишем JavaScript (ES6) красиво.

За основу взята статья ["Airbnb - JavaScript style guide"](https://github.com/airbnb/javascript). Убрано лишнее, нужные моменты переосмысленны и переведены, добавлено некоторые мои видения.

## Оглавление

  1. [Переменные](#Переменные)
  1. [Объекты](#Объекты)
  1. [Массивы](#Массивы)
  1. [Деструктурирование](#Деструктурирование)
  1. [Строки](#Строки)
  1. [Функции](#Функции)
  1. [Стрелочные функции](#Стрелочные-функции)
  1. [Импорты](#Импорты)
  1. [Операторы равенства и идентичности](#Операторы-равенства-и-идентичности)
  1. [Блоки кода](#Блоки-кода)
  
## Переменные

- [1.1](#1.1) <a name='1.1'></a> В ES6 не принято использовать `var`. Используйте `const` если не хотите чтобы значение переменной или ссылка на значение менялось. Во всех остальных случаях используйте `let`.

    eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)
    [`no-var`](http://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)


    > Используя `const` можно предотвратить перезаписи простых типов данных (`string`, `number`, `boolean`, `null`, `undefined`) или перезаписи ссылки на сложные типы данных (`array`, `object`, `function`). Улучшает понимание кода другими программистами, какие переменные можно менять, а какие нет. Отказываясь от `var` получаем везде переменные с блочной областью видимости.
    
    ```javascript
    // bad
    var a = 1;
    var b = 2;
    var count = 0;
    
    if (true) {
      count += 1;
    }
    
    // good
    const a = 1;
    const b = 2;
    let count = 0;
    
    if (true) {
      count += 1;
    }
    ```

- [1.2](#1.2) <a name='1.2'></a> Блочная область видимости `const` и `let`.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

- [1.3](#1.3) <a name='1.3'></a> Всегда используйте `const` или `let` для объявления переменных. Иначе переменная будет объявлена глобально, а это не есть хорошо.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

**[⬆ до оглавления](#Оглавление)**

## Объекты

- [2.1](#2.1) <a name='2.1'></a> Для создания объекта используйте литерал объекта `{}`. Так проще.

    eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html)


    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

- [2.2](#2.2) <a name='2.2'></a> Используйте сокращенный синтаксис для определения методов объекта. 

    eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)


    ```javascript
    // bad
    const item = {
      value: 1,
      addValue: function (value) {
        return this.value + value;
      },
    };

    // good
    const item = {
      value: 1,
      addValue(value) {
        return this.value + value;
      },
    };
    ```

- [2.3](#2.3) <a name='2.3'></a> Используйте сокращенный синтаксис для значений свойств.

    eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)


    ```javascript
    const name = 'Awesome item';

    // bad
    const item = {
      name: name,
    };

    // good
    const item = {
      name,
    };
    ```


    Группируйте сокращенные свойства в начале объекта.


    ```javascript
    const name = 'Awesome item';
    const link = 'https://www.abc.com';

    // bad
    const item = {
      index: 1,
      name,
      type: 'new',
      link
    };

    // good
    const item = {
      name,
      link,
      index: 1,
      type: 'new'
    };
    ```

- [2.4](#2.4) <a name="2.4"></a> Имена свойств не нужно обрамлять в кавычки. Обрамляйте только синтактически неправильные имена (но такие имена лучше не использовать =) ).

    eslint: [`quote-props`](http://eslint.org/docs/rules/quote-props.html) jscs: [`disallowQuotedKeysInObjects`](http://jscs.info/rule/disallowQuotedKeysInObjects)

  ```javascript
  // bad
  const bad = {
    'foo': 3,
    'bar': 4,
    'foo-bar': 5,
  };

  // good
  const good = {
    foo: 3,
    bar: 4,
    'foo-bar': 5,
  };
  ```

- [2.5](#2.5) <a name='2.5'></a> Используйте точечную нотацию для доступа к свойствам объекта.
    eslint: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html) jscs: [`requireDotNotation`](http://jscs.info/rule/requireDotNotation)

    ```javascript
    const item = {
      foo: true,
      bar: 28,
    };

    // bad
    const hasFoo = item['foo'];

    // good
    const hasFoo = item.foo;
    ```

- [2.6](#2.6) <a name='2.6'></a> Используйте скобочную нотацию `[]`, когда вы обращаетесь к свойству через имя хранимое в переменной.

    ```javascript
    const item = {
      foo: true,
      bar: 28,
    };

    function getProp(prop) {
      return item[prop];
    }

    const hasFoo = getProp('foo');
    ```

**[⬆ до оглавления](#Оглавление)**

## Массивы

- [3.1](#3.1) <a name='3.1'></a> Для создания массива используйте литерал массива `[]`. Так проще. 

    eslint: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html)


    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

- [3.2](#3.2) <a name='3.2'></a> Используйте метод `push` для добавления новых элементов.

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

- [3.3](#3.3) <a name='3.3'></a> Используйте оператор расширения `...` для копирование массивов.

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```

## Деструктурирование

- [4.1](#4.1) <a name='4.1'></a> Используйте деструктуризацию объекта в параметрах функции. 

    jscs: [`requireObjectDestructuring`](http://jscs.info/rule/requireObjectDestructuring)


    > Это позволит обойтись без временных переменных.

    ```javascript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

- [4.2](#4.2) <a name='4.2'></a> Используйте деструктуризацию массивов.

    jscs: [`requireArrayDestructuring`](http://jscs.info/rule/requireArrayDestructuring)


    ```javascript
    const arr = [1, 2, 3, 4, 5];

    // bad
    const first = arr[0];
    const second = arr[1];
    const fourth = arr[3];

    // good
    const [first, second, , fourth] = arr;
    ```

- [4.3](#4.3) <a name='4.3'></a> Множество значений возвращайте в виде объекта, чтобы потом использовать деструктуризацию объекта. Это удобней чем возвращать массив.

    > Со временем можно добавлять и возвращать новые переменные не боясь поломать места вызова и использования функции.

    ```javascript
    // bad
    function processInput(input) {
      // calculating...
      return [left, right, top, bottom];
    }

    // the caller needs to think about the order of return data
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // calculating...
      return { left, right, top, bottom };
    }

    // the caller selects only the data they need
    const { left, right } = processInput(input);
    ```

**[⬆ до оглавления](#Оглавление)**

## Строки

- [5.1](#5.1) <a name='5.1'></a> Используйте одинарные кавычки `''` для строк.

    eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)


    ```javascript
    // bad
    const name = "James Bond. James.";

    // good
    const name = 'James. James Bond.';
    ```

- [5.2](#5.2) <a name='5.2'></a> Используйте конкатенацию для переноса длинных строк.

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

- [5.3](#5.3) <a name='5.3'></a> Используйте литералы шаблонов ``, вместо конкатенации, когда программно строите строки.

    eslint: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html) jscs: [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)


    > Шаблоны более читаемы, имеют лаконичный синтаксис, могут быть многострочными и могут интерполироваться. Что нам еще нужно?

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }

    // bad
    const errorMessage = 'This is a super long error that was thrown because \n' +
      'of Batman. When you stop to think about how Batman had anything to do \n' +
      'with this, you would get nowhere fast.';

    // good
    const errorMessage = `This is a super long error that was thrown because 
      of Batman. When you stop to think about how Batman had anything to do 
      with this, you would get nowhere fast.`;
    ```

**[⬆ до оглавления](#Оглавление)**

## Функции

- [6.1](#6.1) <a name='6.1'></a> Старайтесь объявлять именованные функции вместо анонимных (функционального литерала).

    jscs: [`requireFunctionDeclarations`](http://jscs.info/rule/requireFunctionDeclarations)


    > Функциональный литерал создает анонимную функцию, ее сложнее идентифицировать в стеке вызовов в отличие от именованной функции. К тому же все тело именованной фунции поднимается при объявлении, а в случае функционального литерала, поднимается только ссылка на функцию. Следовательно именованную функцию можно использовать даже если в коде она идет ниже чем место вызова.

    ```javascript
    // bad
    const foo = function () {
    };
    foo();

    // good
    function foo() {
    }
    foo();
    
    // still work
    foo();
    function foo() {
    }
    ```

- [6.2](#6.2) <a name='6.2'></a> Используйте значения аргументов по умолчанию, с `ES6` это стало возможно. И не мутируйте аргументы.

    ```javascript
    // really bad
    function handleThings(opts) {
      // No! We shouldn't mutate function arguments.
      // Double bad: if opts is falsy it'll be set to an object which may
      // be what you want but it can introduce subtle bugs.
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      // if opts is undefined, then opts = {}
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

- [6.3](#6.3) <a name='6.3'></a> Значении по умолчанию ставьте в конце списка.

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
      // ...
    }

    // good
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

- [6.4](#6.4) <a name='6.4'></a> Только будьте осторожны с сайд-эффектами.

    ```javascript
    var b = 1;
    // bad
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

- [6.5](#6.5) <a name="6.5"></a> Пробелы в вызовах функции.

    > Единообразность это хорошо, не нужно добавлять или удалять пробел, когда будете добавлять или удалять имя функции.

    ```javascript
    // bad
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // good
    const x = function () {};
    const y = function a() {};
    ```

**[⬆ до оглавления](#Оглавление)**

## Стрелочные функции

- [7.1](#7.1) <a name='7.1'></a> Когда нужно использовать функциональный литерал (например, передача анонимной функции как параметр другой функции), то используйте стрелочные функции.

    eslint: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html) jscs: [`requireArrowFunctions`](http://jscs.info/rule/requireArrowFunctions)


    > Создается функция, которая будет выполняться в контексте текущего `this`, а это то, что нам нужно в большинстве случаях.
    
    > Но если у вас очень сложная функция, то можете вынести эту логику и объявить отдельную функцию.

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

- [7.2](#7.2) <a name='7.2'></a> Разный синтаксис стрелочной функции. 

    [7.2.1](#7.2.1) <a name='7.2.1'></a> Однострочная функция, опускаются фигурные скобки и используется неявный `return`.


    ```javascript
    // one argument
    [1, 2, 3].map(number => `A string containing the ${number}.`);
    
    // to return object include parentheses around it
    [1, 2, 3].map(number => ({num: number, foo: 'bar'}));
    
    // no argument, use parentheses
    [1, 2, 3].map(() => 'No argument');
    
    // two or more arguments, use parentheses
    [1, 2, 3].map((number, index) => 'Two or more arguments');
    ```


    [7.2.2](#7.2.2) <a name='7.2.2'></a> Если выражение не помещается в одну строку, то обрамляем его в круглые скобки.


    ```javascript
    [1, 2, 3].map(number => (
      `As time went by, the string containing the ${number} became much ` +
      'longer. So we needed to break it over multiple lines.'
    ));
    ```


    [7.2.3](#7.2.3) <a name='7.2.3'></a> Многострочная функция, используется круглые скобки для аргументов, фигурные скобки для блок кода и явный `return`.


    ```javascript
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });
    ```

**[⬆ до оглавления](#Оглавление)**

## Импорты

- [8.1](#8.1) <a name='8.1'></a> Первым идет блок импортов стандартных библиотек. Через одну пустую строку идет блок импортов модулей из проекта. Отступаем две строки и пишем код.

    ```javascript
    import React from 'react';
    import _ from 'lodash';
    import {gettext} from 'gettext';
    
    import {view} from './views/index';
    import {model} from './model';
    import {update} from './update';
    
    
    function initApp() {};
    ```

**[⬆ до оглавления](#Оглавление)**

## Операторы равенства и идентичности

- [9.1](#9.1) <a name='9.1'></a> Используйте `===` и `!==` вместо `==` и `!=`.

    eslint: [`eqeqeq`](http://eslint.org/docs/rules/eqeqeq.html)


    > При сравнивании неявно выполняется приведение типов переменных. Чтобы избежать этой путаницы всегда используйте операторы `===` и `!==`, которые сравнивают и значения, и типы выражений.

    ```javascript
    // bad
    false == 0;  // true
    false == ''; // true

    // good
    false === 0;  // false
    false === ''; // false
    ```

- [9.2](#9.2) <a name='9.2'></a> Условные выражения, такие как `if`, вычисляются посредством приведения к логическому типу `Boolean` через метод `ToBoolean`, и всегда следует таким правилам:

    + **Objects** соответствует **true**
    + **Undefined** соответствует **false**
    + **Null** соответствует **false**
    + **Booleans** не меняется
    + **Numbers** соответствует **false** если является **+0, 0, -0 или NaN**, иначе **true**
    + **Strings** соответствует **false** если это пустая строка `''`, иначе **true**


    ```javascript
    if ([0] && []) {
      // true
      // an array (even an empty one) is an object, objects will evaluate to true
    }
    ```

- [9.3](#9.3) <a name='9.3'></a> Используйте короткий синтаксис.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

**[⬆ до оглавления](#Оглавление)**

## Блоки кода

- [10.1](#10.1) <a name='10.1'></a> Используйте фигурные скобки для всех многострочных блоков.

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function foo() { return false; }

    // good
    function bar() {
      return false;
    }
    ```

**[⬆ до оглавления](#Оглавление)**

## Комментарии

- [11.1](#11.1) <a name='11.1'></a> Используйте `/** ... */` для многострочных комментариев.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

- [11.2](#11.2) <a name='11.2'></a> Используйте `//` для однострочного комментария. Размещайте комментарий на новой строке над тем что комментируете. Можете добавить пустую строку над комментарием, чтобы визуально его выделить.

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';
      
      return type;
    }

    // also good
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';
      
      return type;
    }
    ```

**[⬆ до оглавления](#Оглавление)**

## Табы и пробелы

- [12.1](#12.1) <a name='12.1'></a> Используйте программную табуляцию из 4 пробелов (делая `Tab` редактор подставит 4 пробела).

    ```javascript
    // bad
    function foo() {
    ∙const name;
    }

    // bad
    function bar() {
    ∙∙const name;
    }

    // good
    function baz() {
    ∙∙∙∙const name;
    }
    ```

- [12.2](#12.2) <a name='12.2'></a> Поставьте один пробел перед открывающей фигурной скобкой.

    eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks.html) jscs: [`requireSpaceBeforeBlockStatements`](http://jscs.info/rule/requireSpaceBeforeBlockStatements)


    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

- [12.3](#12.3) <a name='12.3'></a> Поставьте один пробел перед открывающей круглой скобкой в операторах управления (`if`, `while`, `for` и т.д.).

    eslint: [`space-after-keywords`](http://eslint.org/docs/rules/space-after-keywords.html), [`space-before-keywords`](http://eslint.org/docs/rules/space-before-keywords.html) jscs:  [`requireSpaceAfterKeywords`](http://jscs.info/rule/requireSpaceAfterKeywords)


    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }
    ```

- [12.4](#12.4) <a name='12.4'></a> Пробелы в вычислениях. 

    eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html) jscs: [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)


    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

- [12.5](#12.5) <a name='12.5'></a> Длинна строки устанавливается в 100 символов, все что длиннее переносим. Допускается выход за границы в разумных пределах.

    > Длинные строки неудобно читать. На современных FullHD мониторах можно расположить два окна рядом по горизонтали. Можно писать и под 80 символов, но это узковато.

    ```javascript
    // bad
    const foo = 'Whatever national crop flips the window. The cartoon reverts within the screw. Whatever wizard constrains a helpful ally. The counterpart ascends!';

    // bad
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // good
    const foo = 'Whatever national crop flips the window. The cartoon reverts within the screw. ' +
      'Whatever wizard constrains a helpful ally. The counterpart ascends!';

    // good
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

**[⬆ до оглавления](#Оглавление)**

## Запятые

- [13.1](#13.1) <a name='13.1'></a> Скажите **нет** запятым в начале строки.

    eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style.html) jscs: [`requireCommaBeforeLineBreak`](http://jscs.info/rule/requireCommaBeforeLineBreak)


    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

- [13.2](#13.2) <a name='13.2'></a> Можно ставить дополнительные запятые в конце объектов и массивов.

    eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle.html) jscs: [`requireTrailingComma`](http://jscs.info/rule/requireTrailingComma)


    > Это улучшит `diff`'ы, меняется одна строка, а не две. Удобней добавлять свойства и элементы, не нужно беспокоиться есть или нет запятые, потому что они всегда есть. 
    
    > Если вы используете Babel — Babel при транскомпиляции игнорирует эти запятые, поэтому можно не беспокоиться об [trailing comma problem](http://stackoverflow.com/questions/7246618/trailing-commas-in-javascript). 
    
    > Если вы не используете Babel — современные браузеры, такие как `Chrome`, `Firefox` адекватно обрабатывают запятые в конце списка. Проблемы появляются только в браузерах `IE` (ниже 9 версии), так что тестируйте и будьте аккуратны.

    ```javascript
    // bad - diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb graph', 'modern nursing']
    };

    // good - diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };

    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];
    ```

**[⬆ до оглавления](#Оглавление)**

## Точки с запятой

- [14.1](#14.1) <a name='14.1'></a> Ставим в конце каждой инструкции.

    eslint: [`semi`](http://eslint.org/docs/rules/semi.html) jscs: [`requireSemicolons`](http://jscs.info/rule/requireSemicolons)


    ```javascript
    // bad
    (function () {
      const name = 'Skywalker'
      return name
    })()

    // good
    (() => {
      const name = 'Skywalker';
      return name;
    }());
    ```

**[⬆ до оглавления](#Оглавление)**

## Соглашение об именовании

- [15.1](#15.1) <a name='15.1'></a> Избегайте однобуквенные имена. Имена должны давать представление о том, что это такое.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

- [15.2](#15.2) <a name='15.2'></a> Используйте `camelCase`, когда именуете объекты, функции и переменные.

    eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html) jscs: [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)


    > Все популярные библиотеки для `javascript` используют `camelCase`, чтобы не путаться и соблюдать единообразие, пишем  в `camelCase`. Исключением являются данные, которые передаются на сервер, там могут быть другие правила.

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

- [15.3](#15.3) <a name='15.3'></a> Используйте `UpperCamelCase`, когда именуете классы или конструкторы классов.

    eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html) jscs: [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)


    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

- [15.4](#15.4) <a name='15.4'></a> Используйте подчеркивание `_` в качестве префикса для именования приватных методов и переменных объекта.

    eslint: [`no-underscore-dangle`](http://eslint.org/docs/rules/no-underscore-dangle.html) jscs: [`disallowDanglingUnderscores`](http://jscs.info/rule/disallowDanglingUnderscores)


    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

- [15.5](#15.5) <a name='15.5'></a> Можете использовать `CONSTANT_CASE` для именования констант.

    ```javascript
    const CAT_INIT_NUMBER_OF_LIVES = 9;
    ```

- [15.6](#15.6) <a name='15.6'></a> В `ES6` можно не сохранять ссылку на `this`. Используйте стрелочные функции или `Function#bind`.

    jscs: [`disallowNodeTypes`](http://jscs.info/rule/disallowNodeTypes)


    ```javascript
    // bad
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

**[⬆ до оглавления](#Оглавление)**
