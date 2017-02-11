## Destructuring 
```js
var foo = { bar: { deep: 'pony', dangerouslySetInnerHTML: 'lol' } }
var {bar: { deep, dangerouslySetInnerHTML: sure }} = foo
```
#### Swapping
```js
var left = 10; var right = 20;
if (right > left) {
  [left, right] = [right, left];
}
```
