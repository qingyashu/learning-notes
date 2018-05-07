# Arrow Function
* Arrow function does not create `this` pointer, `this` inside refers to the one of outer enclosure
* Arrow function does not have `arguments` object, not `new.target` object, nor `super` object. 
* Arrow function allows __rest parameters__
```javascript
// Rest parameters and default parameters are supported
(param1, param2, ...rest) => { statements } 
(param1 = defaultValue1, param2, …, paramN = defaultValueN) => { 
statements } 
```
* Arrow function allows __destructuring assignment__
```javascript
// Destructuring within the parameter list is also supported
var f = ([a, b] = [1, 2], {x: c} = {x: a + b}) => a + b + c; f(); // 6
```

# The "arguments" object
* The `arguments` object is a special object within all functions, it refers to the parameters of that function: 
```javascript
function func1(a, b, c) {
  console.log(arguments[0]);
  // expected output: 1

  console.log(arguments[1]);
  // expected output: 2

  console.log(arguments[2]);
  // expected output: 3
}

func1(1, 2, 3);
```
* `arguments` can be set
```javascript
arguments[1] = 'new value';
```
* `arguments` is like Array object, but does not have other properties except `length` method. 
```javascript
var args = Array.prototype.slice.call(arguments);
var args = [].slice.call(arguments);

// ES2015
const args = Array.from(arguments);
```
* `arguments` object is like `...restArgs` usage, but they have differences: `...restArgs` is normal Array object, `arguments` is whole argument list. 
# 