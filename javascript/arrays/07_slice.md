# slice

- Assigning an array to a new variable doesn't create a copy. Instead, the two variables reference the same array. Any changes to one of them will also show up in the other.

```js
const original = [1, 2, 3];
const copy = original;
copy[0] = 10;
original;
// [10, 2, 3]
```

- Sometimes we want a true copy of an array, so that modifications to the copy don't affect the original. We can use the `.slice` method to do that. If we call `.slice` without any arguments, we get a new array with all elements from the original array.

```js
const original = [1, 2, 3];
const copy = original.slice();
copy;
// [1, 2, 3]
```

```js
const original = [10, 20, 30];
const copy = original.slice();
copy[0] = 1;
original;
// [10, 20, 30]
copy;
// [1, 20, 30]
original[0] = 42;
copy;
// [1, 20, 30]
```

- We can pass two optional arguments to `.slice`. The first is the `start` index, which will give us all elements from that index until the end of the array.

```js
[10, 20, 30, 40, 50].slice(3);
// [40, 50]
```

```js
['a', 'b', 'c'].slice(1);
// ['b', 'c']
```

- The second optional argument is `end`. We'll get all elements from `start` up to `end`, but not including `end`.

```js
[10, 20, 30, 40, 50].slice(1, 2);
// [20]
```

```js
[10, 20, 30, 40, 50].slice(1, 3);
// [20, 30]
```

- We can `.slice` beyond the end of the array. It gives the same result as slicing right up to the last element.

```js
[10, 20].slice(0, 2);
// [10, 20]
```

```js
[10, 20].slice(0, 3);
// [10, 20]
```

- If our `start` index is beyond the end of the array, we get an empty array.

```js
['a', 'b', 'c'].slice(5);
// []
```

```js
[10, 20].slice(2, 3);
// []
```

- Think of it like this. The array `[10, 20]` has indexes 0 and 1. So what's in indexes 2 through 3? There's nothing there. The slice of those indexes is empty.

- There's an important caveat with `.slice`. It creates a new copy of the original array, but it doesn't create copies of the individual elements inside the array.

- For example, the original array might contain a user object. When we copy the array, both arrays reference that same user object. Any changes to the user object are visible in the original and in the copy, since there's only one user.

```js
const original = [{name: 'Amir'}, {name: 'Betty'}];
const copy = original.slice();
copy[0].name = 'Cindy';
original.map(user => user.name);
// ['Cindy', 'Betty']
```

- However, if we store a new user object at index 0, that won't be visible in any copies. The arrays are independent, so completely replacing an element in one doesn't affect the others.

```js
const original = [{name: 'Amir'}, {name: 'Betty'}];
const copy = original.slice();
copy[0] = {name: 'Cindy'};
original.map(user => user.name);
// ['Amir', 'Betty']
```

- We'll explore the issue of copying arrays more deeply in a later lesson. Use .slice when you need to make a copy of an array: either the entire array, or a section of it.

### quiz: "copy array"

Write a function `copyArray` that returns a copy of an array. Changes to the copy shouldn't affect the original array and vice-versa.

Given:

```js
function copyArray(array) {
  return array;
}
```

My answer:

```js
function copyArray(array) {
  return array.slice();
}
```

Author's answer:

```js
function copyArray(array) {
  return array.slice();
}
```

### quiz: "get first elements"

Use `.slice` to write a function `takeFirst(arr, n)`. It should return a new array of the first `n` elements of `arr`. If `n` is larger than the array's length, it should return a copy of the array.

Given: 

```js
function takeFirst(arr, n) {
  return [];
}
```

My answer:

```js
function takeFirst(arr, n) {
  if (n > arr.length) { return arr.slice(); }
  else { return arr.slice(0, n); }
}
```

Author's answer:

```js
function takeFirst(arr, n) {
  return arr.slice(0, n);
}
```

## review #1

```js
['Amir', 'Betty', 'Cindy'].slice(2);
// ['Cindy']
```

## review #2

```js
const original = ['a', 'b', 'c'];
const copy = original.slice();
copy[0] = 'x';
original;
// ['a', 'b', 'c']
```

## review #3

```js
['a', 'b', 'c', 'd', 'e', 'f'].slice(2, 4);
// ['c', 'd']
```

## review #4

```js
['a', 'b', 'c'].slice(2000, 3000);
// []
```

## review #5

```js
[10, 20, 30, 40, 50].slice(-3, -1);
// [30, 40]
```

## review #6

```js
['Amir', 'Betty', 'Cindy', 'Dalili', 'Ebony'].slice(-4, -2);
// ['Betty', 'Cindy']
```

## review #7

```js
[true, true, true, true, false].slice(3);
// [true, false]
```

## review #8

```js
const original = [true, false];
const copy = original.slice();
copy[0] = false;
original;
// [true, false]
```

## review #9

```js
[true, false, true, false].slice(2000000, 3000000);
// []
```

## review #10

```js
[true, false, true, false].slice(0, 3);
// [true, false, true]
```

## review #11

```js
[10, 20].slice(2, 3);
// []
```

## review #12

```js
[10, 20, 30, 40, 50].slice(1, 3);
// [20, 30]
```