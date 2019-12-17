The `**reduce()**` method executes a **reducer** function (that you provide) on each element of the array, resulting in a single output value.

The source for this interactive example is stored in a GitHub repository. If you'd like to contribute to the interactive examples project, please clone [https://github.com/mdn/interactive-examples](https://github.com/mdn/interactive-examples) and send us a pull request.

The **reducer** function takes four arguments:

1.  Accumulator (`acc`)
2.  Current Value (`cur`)
3.  Current Index (`idx`)
4.  Source Array (`src`)

Your **reducer** function's returned value is assigned to the accumulator, whose value is remembered across each iteration throughout the array and ultimately becomes the final, single resulting value.

## Syntax

arr.reduce(callback(accumulator, currentValue\[, index\[, array\]\])\[, initialValue\])

### Parameters

`callback`

A function to execute on each element in the array (except for the first, if no `initialValue` is supplied), taking four arguments:

`accumulator`

The accumulator accumulates the callback's return values. It is the accumulated value previously returned in the last invocation of the callback—or `initialValue`, if it was supplied (see below).

`currentValue`

The current element being processed in the array.

`index` Optional

The index of the current element being processed in the array. Starts from index `0` if an `initialValue` is provided. Otherwise, starts from index `1`.

`array` Optional

The array `reduce()` was called upon.

`initialValue` Optional

A value to use as the first argument to the first call of the `callback`. If no `initialValue` is supplied, the first element in the array will be used and skipped. Calling `reduce()` on an empty array without an `initialValue` will throw a `TypeError`.

### Return value

The single value that results from the reduction.

## Description

The `reduce()` method executes the `callback` once for each assigned value present in the array, taking four arguments:

*   `accumulator`
*   `currentValue`
*   `currentIndex`
*   `array`

The first time the callback is called, `accumulator` and `currentValue` can be one of two values. If `initialValue` is provided in the call to `reduce()`, then `accumulator` will be equal to `initialValue`, and `currentValue` will be equal to the first value in the array. If no `initialValue` is provided, then `accumulator` will be equal to the first value in the array, and `currentValue` will be equal to the second.

**Note:** If `initialValue` is not provided, `reduce()` will execute the callback function starting at index 1, skipping the first index. If `initialValue` is provided, it will start at index 0.

If the array is empty and no `initialValue` is provided, [`TypeError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError "The TypeError object represents an error when an operation could not be performed, typically (but not exclusively) when a value is not of the expected type.") will be thrown.

If the array only has one element (regardless of position) and no `initialValue` is provided, or if `initialValue` is provided but the array is empty, the solo value will be returned _without_ calling _`callback`._

It is usually safer to provide an `initialValue`, because there are three possible outputs without `initialValue`, as shown in the following example.

```
let maxCallback = ( acc, cur ) => Math.max( acc.x, cur.x )
let maxCallback2 = ( max, cur ) => Math.max( max, cur )

// reduce() without initialValue
[ { x: 22 }, { x: 42 } ].reduce( maxCallback ) // 42
[ { x: 22 }            ].reduce( maxCallback ) // { x: 22 }
[                      ].reduce( maxCallback ) // TypeError

// map/reduce; better solution, also works for empty or larger arrays
[ { x: 22 }, { x: 42 } ].map( el => el.x )
                        .reduce( maxCallback2, -Infinity )

```

### How reduce() works

Suppose the following use of `reduce()` occurred:

```
[0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array) {
  return accumulator + currentValue
})

```

The callback would be invoked four times, with the arguments and return values in each call being as follows:

| `callback` | `accumulator` | `currentValue` | `currentIndex` | `array` | return value |
| --- | --- | --- | --- | --- | --- |
| first call | `0` | `1` | `1` | `[0, 1, 2, 3, 4]` | `1` |
| second call | `1` | `2` | `2` | `[0, 1, 2, 3, 4]` | `3` |
| third call | `3` | `3` | `3` | `[0, 1, 2, 3, 4]` | `6` |
| fourth call | `6` | `4` | `4` | `[0, 1, 2, 3, 4]` | `10` |

The value returned by `reduce()` would be that of the last callback invocation (`10`).

You can also provide an [Arrow Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions "An arrow function expression is a syntactically compact alternative to a regular function expression, although without its own bindings to the this, arguments, super, or new.target keywords. Arrow function expressions are ill suited as methods, and they cannot be used as constructors.") instead of a full function. The code below will produce the same output as the code in the block above:

```
[0, 1, 2, 3, 4].reduce( (accumulator, currentValue, currentIndex, array) => accumulator + currentValue );

```

If you were to provide an `initialValue` as the second argument to `reduce()`, the result would look like this:

```
[0, 1, 2, 3, 4].reduce((accumulator, currentValue, currentIndex, array) => {
    return accumulator + currentValue;
}, 10);

```

| `callback` | `accumulator` | `currentValue` | `currentIndex` | `array` | return value |
| --- | --- | --- | --- | --- | --- |
| first call | `10` | `0` | `0` | `[0, 1, 2, 3, 4]` | `10` |
| second call | `10` | `1` | `1` | `[0, 1, 2, 3, 4]` | `11` |
| third call | `11` | `2` | `2` | `[0, 1, 2, 3, 4]` | `13` |
| fourth call | `13` | `3` | `3` | `[0, 1, 2, 3, 4]` | `16` |
| fifth call | `16` | `4` | `4` | `[0, 1, 2, 3, 4]` | `20` |

The value returned by `reduce()` in this case would be `20`.

## Examples

### Sum all the values of an array

```
let sum = [0, 1, 2, 3].reduce(function (accumulator, currentValue) {
  return accumulator + currentValue
}, 0)
// sum is 6


```

Alternatively written with an arrow function:

```
let total = [ 0, 1, 2, 3 ].reduce(
  ( accumulator, currentValue ) => accumulator + currentValue,
  0
)
```

### Sum of values in an object array

To sum up the values contained in an array of objects, you **must** supply an `initialValue`, so that each item passes through your function.

```
let initialValue = 0
let sum = [{x: 1}, {x: 2}, {x: 3}].reduce(function (accumulator, currentValue) {
    return accumulator + currentValue.x
}, initialValue)

console.log(sum) // logs 6

```

Alternatively written with an arrow function:

```
let initialValue = 0
let sum = [{x: 1}, {x: 2}, {x: 3}].reduce(
    (accumulator, currentValue) => accumulator + currentValue.x
    , initialValue
)

console.log(sum) // logs 6
```

### Flatten an array of arrays

```
let flattened = [[0, 1], [2, 3], [4, 5]].reduce(
  function(accumulator, currentValue) {
    return accumulator.concat(currentValue)
  },
  []
)
// flattened is [0, 1, 2, 3, 4, 5]

```

Alternatively written with an arrow function:

```
let flattened = [[0, 1], [2, 3], [4, 5]].reduce(
  ( accumulator, currentValue ) => accumulator.concat(currentValue),
  []
)

```

### Counting instances of values in an object

```
let names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice']

let countedNames = names.reduce(function (allNames, name) { 
  if (name in allNames) {
    allNames[name]++
  }
  else {
    allNames[name] = 1
  }
  return allNames
}, {})
// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }

```

### Grouping objects by a property

```
let people = [
  { name: 'Alice', age: 21 },
  { name: 'Max', age: 20 },
  { name: 'Jane', age: 20 }
];

function groupBy(objectArray, property) {
  return objectArray.reduce(function (acc, obj) {
    var key = obj[property];
    if (!acc[key]) {
      acc[key] = []
    }
    acc[key].push(obj)
    return acc
  }, {})
}

let groupedPeople = groupBy(people, 'age')
// groupedPeople is:
// { 
//   20: [
//     { name: 'Max', age: 20 }, 
//     { name: 'Jane', age: 20 }
//   ], 
//   21: [{ name: 'Alice', age: 21 }] 
// }

```

### Bonding arrays contained in an array of objects using the spread operator and initialValue

```
// friends - an array of objects 
// where object field "books" is a list of favorite books 
let friends = [{
  name: 'Anna',
  books: ['Bible', 'Harry Potter'],
  age: 21
}, {
  name: 'Bob',
  books: ['War and peace', 'Romeo and Juliet'],
  age: 26
}, {
  name: 'Alice',
  books: ['The Lord of the Rings', 'The Shining'],
  age: 18
}]

// allbooks - list which will contain all friends' books +  
// additional list contained in initialValue
let allbooks = friends.reduce(function(accumulator, currentValue) {
  return [...accumulator, ...currentValue.books]
}, ['Alphabet'])

// allbooks = [
//   'Alphabet', 'Bible', 'Harry Potter', 'War and peace', 
//   'Romeo and Juliet', 'The Lord of the Rings',
//   'The Shining'
// ]
```

### Remove duplicate items in array

**Note:** If you are using an environment compatible with [`Set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set "The Set object lets you store unique values of any type, whether primitive values or object references.") and [`Array.from()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from "The Array.from() method creates a new, shallow-copied Array instance from an array-like or iterable object."), you could use `let orderedArray = Array.from(new Set(myArray))` to get an array where duplicate items have been removed.

```
let myArray = ['a', 'b', 'a', 'b', 'c', 'e', 'e', 'c', 'd', 'd', 'd', 'd']
let myOrderedArray = myArray.reduce(function (accumulator, currentValue) {
  if (accumulator.indexOf(currentValue) === -1) {
    accumulator.push(currentValue);
  }
  return accumulator
}, [])

console.log(myOrderedArray);
```

### Running Promises in Sequence

```
/**
 * Runs promises from array of functions that can return promises
 * in chained manner
 *
 * @param {array} arr - promise arr
 * @return {Object} promise object
 */
function runPromiseInSequence(arr, input) {
  return arr.reduce(
    (promiseChain, currentFunction) => promiseChain.then(currentFunction),
    Promise.resolve(input)
  )
}

// promise function 1
function p1(a) {
  return new Promise((resolve, reject) => {
    resolve(a * 5)
  })
}

// promise function 2
function p2(a) {
  return new Promise((resolve, reject) => {
    resolve(a * 2)
  })
}

// function 3  - will be wrapped in a resolved promise by .then()
function f3(a) {
 return a * 3
}

// promise function 4
function p4(a) {
  return new Promise((resolve, reject) => {
    resolve(a * 4)
  })
}

const promiseArr = [p1, p2, f3, p4]
runPromiseInSequence(promiseArr, 10)
  .then(console.log)   // 1200

```

### Function composition enabling piping

```
// Building-blocks to use for composition
const double = x => x + x
const triple = x => 3 * x
const quadruple = x => 4 * x

// Function composition enabling pipe functionality
const pipe = (...functions) => input => functions.reduce(
    (acc, fn) => fn(acc),
    input
)

// Composed functions for multiplication of specific values
const multiply6 = pipe(double, triple)
const multiply9 = pipe(triple, triple)
const multiply16 = pipe(quadruple, quadruple)
const multiply24 = pipe(double, triple, quadruple)

// Usage
multiply6(6) // 36
multiply9(9) // 81
multiply16(16) // 256
multiply24(10) // 240


```

### write map using reduce

```
if (!Array.prototype.mapUsingReduce) {
  Array.prototype.mapUsingReduce = function(callback, thisArg) {
    return this.reduce(function(mappedArray, currentValue, index, array) {
      mappedArray[index] = callback.call(thisArg, currentValue, index, array)
      return mappedArray
    }, [])
  }
}

[1, 2, , 3].mapUsingReduce(
  (currentValue, index, array) => currentValue + index + array.length
) // [5, 7, , 10]


```

## Polyfill

```
// Production steps of ECMA-262, Edition 5, 15.4.4.21
// Reference: http://es5.github.io/#x15.4.4.21
// https://tc39.github.io/ecma262/#sec-array.prototype.reduce
if (!Array.prototype.reduce) {
  Object.defineProperty(Array.prototype, 'reduce', {
    value: function(callback /*, initialValue*/) {
      if (this === null) {
        throw new TypeError( 'Array.prototype.reduce ' + 
          'called on null or undefined' );
      }
      if (typeof callback !== 'function') {
        throw new TypeError( callback +
          ' is not a function');
      }

      // 1. Let O be ? ToObject(this value).
      var o = Object(this);

      // 2. Let len be ? ToLength(? Get(O, "length")).
      var len = o.length >>> 0; 

      // Steps 3, 4, 5, 6, 7      
      var k = 0; 
      var value;

      if (arguments.length >= 2) {
        value = arguments[1];
      } else {
        while (k < len && !(k in o)) {
          k++; 
        }

        // 3. If len is 0 and initialValue is not present,
        //    throw a TypeError exception.
        if (k >= len) {
          throw new TypeError( 'Reduce of empty array ' +
            'with no initial value' );
        }
        value = o[k++];
      }

      // 8. Repeat, while k < len
      while (k < len) {
        // a. Let Pk be ! ToString(k).
        // b. Let kPresent be ? HasProperty(O, Pk).
        // c. If kPresent is true, then
        //    i.  Let kValue be ? Get(O, Pk).
        //    ii. Let accumulator be ? Call(
        //          callbackfn, undefined,
        //          « accumulator, kValue, k, O »).
        if (k in o) {
          value = callback(value, o[k], k, o);
        }

        // d. Increase k by 1.      
        k++;
      }

      // 9. Return accumulator.
      return value;
    }
  });
}

```

**Caution:** If you need to support truly obsolete JavaScript engines that do not support `[Object.defineProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)`, it is best not to polyfill `Array.prototype` methods at all, as you cannot make them **non-enumerable**.

## Specifications

| Specification | Status | Comment |
| --- | --- | --- |
| [ECMAScript 5.1 (ECMA-262)  <br>The definition of 'Array.prototype.reduce()' in that specification.](https://www.ecma-international.org/ecma-262/5.1/#sec-15.4.4.21) | Standard | Initial definition. Implemented in JavaScript 1.8. |
| [ECMAScript 2015 (6th Edition, ECMA-262)  <br>The definition of 'Array.prototype.reduce()' in that specification.](https://www.ecma-international.org/ecma-262/6.0/#sec-array.prototype.reduce) | Standard |     |
| [ECMAScript Latest Draft (ECMA-262)  <br>The definition of 'Array.prototype.reduce()' in that specification.](https://tc39.es/ecma262/#sec-array.prototype.reduce) | Draft |     |

## Browser compatibility

The compatibility table in this page is generated from structured data. If you'd like to contribute to the data, please check out [https://github.com/mdn/browser-compat-data](https://github.com/mdn/browser-compat-data) and send us a pull request.