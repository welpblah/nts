# arrays: element access

- Arrays are sequences of values that have a specific order and a length.

```javascript
['a', 'b', 'c'].length
// 3
```

```javascript
[].length
// 0
```

- Each element in the array gets a sequential index, starting at 0. (We say that arrays are "zero-indexed".) We can retrieve an element by providing its index in [square brackets].

```javascript
const strings = ['a', 'b', 'c'];
strings[0];
// 'a'
```

```javascript
const strings = ['a', 'b', 'c'];
strings[2];
// 'c'
```

- If we try to access an index that's not in the array, we get `undefined`.

```javascript
const strings = ['a', 'b', 'c'];
strings[14];
// undefined
```

- After we've accessed an element with `[]`, we can assign it a new value with `=`.

```javascript
const strings = ['a', 'b', 'c'];
strings[2] = 'x';
strings;
// ['a', 'b', 'x']
```

- In some languages, an array can only contain elements of a single type. However, JavaScript arrays can contain a mix of numbers, strings, objects, and even other arrays.

```javascript
const kitchenSink = ['a', 42, {name: 'Amir'}, ['Betty']];
kitchenSink[3];
/// ['Betty']
```

- When arrays contain other arrays, we can access elements by providing an index for each array in order, like `someArray[1][0]`. An array containing arrays is sometimes called a "two-dimensional" array. For example, we might use a two-dimensional array to represent cartesian coordinates in a plane.

```javascript
const linePath = [[1, 3], [2, 4], [8, 14]];
const point1 = linePath[1];
point1[0];
// 2
```

```javascript
const linePath = [[1, 3], [2, 4], [8, 14]];
linePath[1][0];
// 2
```

```javascript
const linePath = [[1, 3], [2, 4], [8, 14]];
linePath[2][1];
// 14
```

---

## quiz: element assignment

Write a function that puts a `0` in element 1 of an array. Remember that arrays are zero-indexed.

Make sure that the function returns `undefined`. Returning `undefined` indicates that the function modifies some data: either its arguments or global variables.

(If a function returns nothing, but also modifies nothing, then it does nothing at all! It must return something, modify something, or both. By returning `undefined`, we tell callers of our function that it modifies something.)

Given:

```javascript
function setSecondElementToZero(nums) {
}
```

My answer:

```javascript
function setSecondElementToZero(nums) {
  if (nums[1] === 0) { return undefined; }
  return nums[1] = 0;
}
```

Author's answer:

```javascript
function setSecondElementToZero(nums) {
  nums[1] = 0;
  return undefined;
}
```

## quiz: element access

Write a function `at(arr, i)` that returns the element at index `i`. The function should return `null` if the index isn't in the array, for example in `at([], 1)`.

One way to do this is to compare the requested index against 0 and the array's length.

Given:

```javascript
function at(arr, i) {
  return arr[i];
}
```

My answer:

```javascript
function at(arr, i) {
  if(i in arr) {
    return arr[i];
  } else {
    return null
  }
  return undefined;
}
```

Author's answer

```javascript
  // Is this index actually in the array?
  if (i < 0 || i >= arr.length) {
    return null;
  } else {
    return arr[i];
  }
```

## quiz: empty

Write a function `isEmpty(arr)` that returns `true` if the array is empty.

Given:

```javascript
function isEmpty(arr) {
  return true;
}
```

My answer:

```javascript
function isEmpty(arr) {
  return arr.length <= 0;
}
```

Author's answer:

```javascript
function isEmpty(arr) {
  return arr.length === 0;
}
```

---

## review

```js
const strings = ['a', 'b', 'c'];
strings[0] = 'x';
strings;
// ['x', 'b', 'c']
```

```js
const linePath = [[1, 3], [2, 4], [8, 14]];
linePath[1][0];
// 2
```

```js
const strings = ['a', 'b', 'c'];
strings[1] = 'x';
strings;
// ['a', 'x', 'c']
```

```js
const linePath = [[1, 3], [2, 4], [8, 14]];
linePath[2][1];
// 14
```
