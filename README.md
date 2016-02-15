# javascriptstyleguide
Пишим JavaScript (es6) красиво.

За основу взята статья ["Airbnb javascript style guide"](https://github.com/airbnb/javascript). Убранно то что я считаю лишним, нужные моменты переосмысленны и переведенны, добавленны некоторые мои видения.

## Переменные var, const, let

- [1.1](#1.1) <a name='1.1'></a> Используйте `const` для всех переменных. В es6 не принято использовать `var`.

    eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)
    [`no-var`](http://eslint.org/docs/rules/no-var.html)

    jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

> Why? This ensures that you can't reassign your references, which can lead to bugs and difficult to comprehend code.

```javascript
// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;
```
