# javascriptstyleguide
Пишим JavaScript (es6) красиво.
За основу принята статья ["Airbnb javascript style guide"](https://github.com/airbnb/javascript) <a name='1.1'></a>

## Переменные var, const, let

- [1.1](#1.1) <a name='1.1'></a> Используйте `const` для всех переменных. В es6 не принято использовать `var`. eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)

> Why? This ensures that you can't reassign your references, which can lead to bugs and difficult to comprehend code.

```javascript
// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;
```
