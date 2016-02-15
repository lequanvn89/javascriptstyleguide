# javascriptstyleguide
Пишим JavaScript (ES6) красиво.

За основу взята статья ["Airbnb - JavaScript style guide"](https://github.com/airbnb/javascript). Убранно то что я считаю лишним, нужные моменты переосмысленны и переведенны, добавленны некоторые мои видения.

## Оглавление

  1. [Переменные](#Переменные)
  1. [Объекты](#Объекты)
  
## Переменные

- [1.1](#1.1) <a name='1.1'></a> В ES6 не принято использовать `var`. Используйте `const` если не хотите чтобы ссылка переменная-значение менялась. Во всех остальных случаях используйте `let`.

    eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)
    [`no-var`](http://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

    > Используя `const` можно предовратить перезаписи простых типов данных (`string`, `number`, `boolean`, `null`, `undefined`) или перезаписи ссылки на сложные типы данных (`array`, `object`, `function`). Улучшает понимание кода другим программистам, какие переменные можно трогать, а какие нет. Отказываясь от `var` получаем везде переменные с блочной областью видимости.
    
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

- [1.2](#1.2) <a name='1.2'></a> Блочная обасть видимости `const` и `let`.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```
    
**[⬆ до оглавления](#Оглавление)**

## Объекты

- [2.1](#2.1) <a name='2.1'></a> Для создания объекта используйте литерал объекта (фигурные скобки). Так проще.

    eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

- [2.2](#2.2) <a name='2.2'></a> Используйте сокращенный синтаксис для объявления методов объекта. 

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
