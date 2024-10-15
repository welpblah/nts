# find index

- We've seen that the `.indexOf` method gives us the index of an element.

```js
['a', 'b', 'c'].indexOf('b');
// 1
```

- Sometimes we want to match a condition, rather than looking for a specific element value. For example, we might want to find the first non-empty string.

- `.findIndex` does exactly that: it's like `.indexOf`, but it takes a function instead of an element value. The function is called on every element. We get the first element where the function returns `true`.

```js
['', 'a'].findIndex(string => string.length !== 0);
// 1
```

```js
const users = [
  {name: 'Amir', admin: false},
  {name: 'Betty', admin: true},
];
users.findIndex(user => user.admin === true);
// 2
```

- Like `.indexOf`, `.findIndex` returns the index of the first matching element, even if multiple elements match. The next example finds the index of the first 'a'.

```js
['a', 'b', 'a', 'b'].findIndex(string => string === 'a');
// 0
```

- The callback function passed to .findIndex gets three arguments. Usually we only need the first, which is the current array element's value. Sometimes we also need the element's index, which we get as the second callback argument.

- The next example finds the index of the first `'a'` whose index is greater than 1.

```js
['a', 'c', 'b', 'a'].findIndex(
(elem, index) => index > 1 && elem === 'a');
// 3
```

- The third callback argument is the entire array. This is rarely needed, but it's there just in case.

```js
['a', 'b', 'c'].findIndex(
  (element, index, array) => array[index] === 'c'
);
// 2
```

- If our function doesn't match any elements, `.findIndex` returns `-1`, like `.indexOf` does. As with `.indexOf`, this can be a source of bugs. Remember to check for `-1`!

```js
[false, true].findIndex(element => element === true);
// 1
```

```js
[false, true].findIndex(element => element === null);
// -1
```

### quiz: "implement find"

Use `.findIndex` to write the function `find(arr, callback)`. It should return the first element where `callback(element)` is true. If the element is not found, it should return `undefined`.

Hint: `.findIndex` returns -1 when an element isn't found.

Given:

```js
function find(arr, callback) {
  const index = 0;
  return arr[index];
}
```

My answer:

```js
function find(arr, callback) {
  const index = arr.findIndex(callback);
  if (index === -1) {
    return undefined;
  } else {
    return arr[index];
  }
}
```

Author's answer:

```js
function find(arr, callback) {
  const index = arr.findIndex(callback);
  if (index === -1) {
    return undefined;
  } else {
    return arr[index];
  }
}
```

## review #1

```js
[true, false, true, false].findIndex(bool => !bool);
// 1
```

## review #2

```js
['a', 'b', 'c'].findIndex(element => element === 'd');
// -1
```

## review #3

```js
['Amir', 'Betty', 'Cindy', 'Dalili'].findIndex(name => name.startsWith('D'));
// 3
```

## review #4

```js
[1000, 2000, 3000].findIndex(element => element < 1000);
// -1
```

## review #5

```js
[false, true].findIndex(element => element === null);
// -1
```