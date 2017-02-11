## Destructuring 
Good for : 
- read object returned by function
- default function parameter value ```function random ({ min=1, max=300 } = {}) {}```
- [regexp extraction](https://ponyfoo.com/articles/es6-destructuring-in-depth#use-cases-for-destructuring)
```js
var [a] = [10]
var [,,a,b] = [1,2,3,4,5]
var {foo: a, bar: b} = {foo: 1, bar: 2}

var foo = { bar: { deep: 'pony', dangerouslySetInnerHTML: 'lol' } }
var {bar: { deep, dangerouslySetInnerHTML: sure }} = foo

var key = 'dynamic'
var { [key]: foo } = { key1: 'foo' }  // foo == 'foo'

//// Swapping
var left = 10; var right = 20;
if (right > left) {
  [left, right] = [right, left];
}

//// Default values
var {foo=3} = { bar: 2 }

//// Function parameters
function greet ({ age, name:greeting='she' }) {
  console.log(`${greeting} is ${age} years old.`)
}
greet({ name: 'nico', age: 27 })
```
