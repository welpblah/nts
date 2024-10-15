# shift

- We've seen `.push` and `.pop`. They add and remove elements on the end of the array.

- `.shift` and `.unshift` do the same, but for the beginning of the array. `.shift` removes the first element of the array and returns it.

```js
const a = ['a', 'b'];
a.shift();
a;
// ['b']
```

```js
const a = ['a', 'b'];
a.shift();
// 'a'
```

- `.unshift` adds an element to the beginning of an array.

```js
const a = ['a', 'b'];
a.unshift('z');
a;
// ['z', 'a', 'b']
```

- `.unshift` returns the array's length, including the newly-added element. This matches `.push`, which also returns the array's length.

```js
const a = ['a', 'b'];
a.unshift('z');
// 3
```

- `.shift` and `.unshift` have somewhat confusing names. Here's one way to remember them. `.shift` "shifts" all of the elements down by one. Index 2 becomes index 1; index 1 becomes index 0; etc. `.unshift` does the opposite.

Here's a demonstration.

```js
const a = ['a', 'b', 'c'];
const origIndex = a.indexOf('c');
a.shift();
const newIndex = a.indexOf('c');
origIndex + ' becomes ' +  newIndex;
// '2 becomes 1'
```

- Calling `.unshift` puts the new element in index 0. This means that the existing elements all move to the right by one. Their indexes all increase.

```js
const a = ['a', 'b'];
const origIndex = a.indexOf('b');
a.unshift('z');
const newIndex = a.indexOf('b');
origIndex + ' becomes ' + newIndex;
// '1 becomes 2'
```

- Like `.pop` and `.push`, `.shift` and `.unshift` change the original array.

```js
const a = [1, 2, 3];
a.shift();
a.shift();
a;
// [3]
```

```js
const a = [1, 2, 3];
a.shift();
a.unshift('a');
a;
// ['a', 2, 3]
```

### quiz: "rotate right"

Write a `rotate` function. It should move the last element to the beginning of the array. The function should modify the original array.

You'll need to handle the special case when an array is empty (with a `length` of 0.) When `rotate` gets an empty array, it should also return an empty array.

Given:

```js
function rotate(arr) {
  return arr;
}
```

My answer:

```js
function rotate(arr) {
  if (arr.length === 0) { return arr; }

  const lastElement = arr.pop();
  arr.unshift(lastElement);
  
  return arr;
}
```

Author's answer:

```js
function rotate(arr) {
  if (arr.length === 0) {
    return arr;
  } else {
    arr.unshift(arr.pop());
  }
}
```

### review

```js
const a = ['Amir', 'Betty', 'Cindy'];
a.shift();
// ['Amir']
a;
// ['Betty', 'Cindy']
```

```js
const a = ['Amir', 'Betty'];
a.unshift('Cindy');
a;
// ['Cindy', 'Amir', 'Betty']
```

```js
const a = [true, false];
a.shift();
a;
// [false]
```

```js
const a = [true, false];
a.unshift(true);
a;
// [true, true, false]
```
