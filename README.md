An ES6 cheet sheet made after following this excellent [ES6 tour](https://ponyfoo.com/articles/tagged/es6-in-depth). Written to be useful especially for Scala/Java/C# dev doing a bit of JS on the side (by choice... or more likely because of projects constraints).

## Let / Const
Block-scoped declarations. Avoid ```var``` and use the newer keywords ```const``` if possible, ```let``` if not.
```js
const immutableRef = 3
let mutableRef = 4
```
Note : Be mindful of the [Temporal Dead Zone](http://jsrocks.org/2015/01/temporal-dead-zone-tdz-demystified/) and [hoisting](https://ponyfoo.com/articles/javascript-variable-hoisting)

## Classes
```js
class Car {
  constructor (startDistance) {
    this.distance = startDistance
  }
  move () {
    this.distance += 2	
  }
  static isFarther (left, right) {
    return left.distance > right.distance
  }
}

//// Inheritance
class FastCar extends Car {
  constructor () {
    super(0)
  }
	move () {
    super.move()
    this.distance += 4
  }
}
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

//// Raw template
var singleLine = String.raw`The "\n" newline won't result in a new line.`
```
NOTE : you can create your own template functions, usable in the same fashion as String.raw , see [here](https://ponyfoo.com/articles/es6-template-strings-in-depth#demystifying-tagged-templates)

## Collections
```js
////Maps : Iterable, accept any key
var map = new Map()
map.set('b', { desc: 'letter b'})
map.set(2, 'the number 2')
map.set({ name: 'first object'}, 45)

map.get('b')
map.delete(2)
map.has(2)
map.clear()
map.size

var map = new Map([[1, 'a'], [2, 'b']])
for (let [key, value] of map) {
  console.log(`${key}: ${value}`)
}

//// WeakMaps : Not Iterable, only object keys
var map = new WeakMap()
map.set({ id : 1 })

//// Set : Iterable, no duplicate values
var set = new Set([1, 2, 3, 4, 4])
set.add(5)

//// Weakset : Not Iterable, only object values
var set = new Weakset()
set.add({})
```

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
## Iterators
Applies on :  
- Arrays, 
- Objects with [Symbol.iterator] method, 
- Generators
- DOM node collections from .querySelectorAll
```js
for (let elt of elements) {}

//// Iterators are lazy (can run forever if iterable is infinite)
for (let elt of elements) {
  if (elt === 'a') { break; }
}
```
## Generators
Generates value readable by ```Array.from(g)```, ```[...g]```, or ```for value of g```
```js
function* generator () {
  yield 1
  yield 2
  yield 'abcdef'
}
var g = generator()

for (value in g) { console.log(value); }

console.log([...g]

//// Manually
while (true) {
  let item = g.next()
  if (item.done) { break; }
  console.log(item.value)
}

//// Revert responsibility "the generator defines what to iterate over, not the how"
function power (factors) {
  var g = factors()
  while (true) {
    let factor = g.next()
    if (factor.done) {
      break
    }
    console.log(Math.pow(2, factor.value)
  }
}

power(function* factors () { yield 2; yield 5; })

//// Throw and Return
g.throw(err) // Throw an error on the current 'yield' level
g.return()   // End the sequence, behave like an exception => try/catch possible
```
## Arrow Functions
```js
[1, 2, 3, 4].map((num, index) => num * 2 + index)

[1, 2, 3, 4].map(num => {
  var multiplier = 2 + num
  return num * multiplier
})
```
Note: Careful with their [Lexical Scope](https://derickbailey.com/2015/09/28/do-es6-arrow-functions-really-solve-this-in-javascript/) !

## Object literals
```js
//// Property value shorthand
function clear () { elt = {} }
module.exports = { clear }

function getCar(make, model) {
	return {
		make,  // same as make: make
		model, // same as model: model
		['make' + make]: true,

    // omits `function` keyword & colon
		depreciate() {
			this.value -= 2500;
		}
}
```

## Rest parameters
```js
function sum (multiplier, base, ...numbers) {
  var sum = numbers.reduce((accumulator, num) => accumulator + num, base)
  return multiplier * sum
}
```

## Spread operator
| Use Case       | ES6                       | ES5 |
| -------------  | -------------             | ------------- |
| Destructuring  | ```[a, ...rest] = list``` | ```a = list[0], rest = list.slice(1)```  |
| Concatenation  | ```[1, 2, ...more]```     |```[1, 2].concat(more)```  | 
| Push onto list | ```list.push(...[3, 4])```| ```list.push.apply(list, [3, 4])```  |

## Symbols
```js
var foo = {
  [Symbol()]: 'foo',
  [Symbol('foo')]: 'bar',
  [Symbol.for('bar')]: 'baz',
}
```
## Proxies
```js
var target = { prop: 2 }
var handler = {
  get (target, key) {
	  console.log(`accessed var $key`)
    return target[key]
  },
  set (target, key, value) {
    // block modification
    return false
  }
}
var proxy = new Proxy(target, handler)

var {proxy, revoke} = Proxy.revocable(target, handler)
revoke()
```
Much more on proxies [here](https://ponyfoo.com/articles/es6-proxy-traps-in-depth)

## Numbers

```js
Number.isInteger(2)
Number.isSafeInteger()
Number.EPSILON
```

## Arrays
```js
Array.from($('div'))  // <- [<div>, <div>, <div>, ...]
Array.from(arguments, value => typeof value)

Array.of(1, 2, 3)

['a', 'b', 'c'].fill(0)
['a', 'b', 'c',,,].fill(0, 2)
new Array(3).fill({})

[1, 2, 3, 4, 5].find((item, i) => i === 3)

[1, 2, 3].keys()
for (let key of [1, 2, 3].keys()) {
  console.log(key)
  // <- 0
  // <- 1
  // <- 2
}
```
