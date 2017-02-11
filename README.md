## Destructuring 
```js
var [a] = [10]
var {foo: a, bar: b} = {foo: 1, bar: 2}
// a == 1 , b == 2

var foo = { bar: { deep: 'pony', dangerouslySetInnerHTML: 'lol' } }
var {bar: { deep, dangerouslySetInnerHTML: sure }} = foo

var key = 'dynamic'
var { [key]: foo } = { key1: 'foo' }
// foo == 'foo'

//// Swapping
var left = 10; var right = 20;
if (right > left) {
  [left, right] = [right, left];
}

//// Default values
var {foo=3} = { bar: 2 }
// foo == 3
```
