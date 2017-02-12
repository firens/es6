An ES6 cheet sheet made after following this excellent [ES6 tour](https://ponyfoo.com/articles/tagged/es6-in-depth).

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

## Template literals
```js
var {count, today} = { count: 3, names: ['john', 'joe', 'josh'], today: new Date()}
var html = `<p>
  I'm "happy" to have ${count} brothers, today ${today.toLocaleString()}
  They are called
</p>
<ul>
  ${names.map(name => `<li>${name}</li>`).join('\n')}
</ul>
`

// Raw template
var singleLine = String.raw`The "\n" newline won't result in a new line.`
```
NOTE : you can create your own template functions, usable in the same fashion as String.raw , see [here](https://ponyfoo.com/articles/es6-template-strings-in-depth#demystifying-tagged-templates)
