# arrays: some and every

- The `.some` tells us whether a function is true for any element in an array. For example: "are any of these elements `2`?" Or "are any of these elements an empty string?"

- `.some` takes a callback function that runs on every element. If the callback returns `true` for any element, `.some` returns `true`.

```js
[1, 2, 3].some(n => n === 2);
// true
```

```js
['a', 'bc', 'def'].some(
  string => string.length === 0);
// false
```

- `.some` returns `false` for an empty array. This makes some sense if we think through it.

Suppose that we want to know whether some people are named "Amir". If we ask this about Amir, the answer is yes. If we ask this about Betty, the answer is no. If we ask it about nobody at all, the answer is no; there's no Amir.

```js
[].some(user => user.name === 'Amir');
// false
```

- To check for whether every element matches some condition, we use `.every`. It works just like `.some`, but it only returns `true` if our function returns `true` for every element in the array.

```js
['a', 'bc', 'def'].every(string => string.length > 0);
// true
```

```js
[9, 10, 11].every(n => n >= 10);
// false
```

- `.every` always returns `true` for an empty array. One way to remember this is that `.every`'s behavior for empty arrays is the opposite of `.some`'s behavior.

	- `[].every(f)` is `true`, no matter what `f` is.
	- `[].some(f)` is `false`, no matter what `f` is.

```js
[].every(user => user.name === 'Amir');
// true
```

- Sometimes we want to confirm that an array doesn't include a specific element. While there's no built-in way to do this, we can negate `.some` to achieve the same result.

```js
!['a', 'bc', 'def'].some(string => string.length === 0);
// true
```

- We can take this one step further and define our own `none` function. It's a bit easier to think about `none` than `![...].some`.

```js
function none(array, callback) {
  return !array.some(callback);
}
none(['a', 'bc', 'def'], string => string.length === 0);
// true
```

---

## quiz: "implement none"

Use `.some` to write the function `none`. It should take an array and a callback, and return `true` if the callback is `false` for every element.

Given:

```js
function none(arr, callback) {
  return true;
}
```

My answer:

```js
function none(arr, callback) {
  return !(arr.some(callback));
}
```

Author's answer:

```js
function none(arr, callback) {
  return !arr.some(callback);
}
```

### quiz: "has null"

Write a function `hasNull` that returns `true` if an array contains `null`.

A hint: `null == undefined` (this is a "double equals" comparison). But `null !== undefined` (this is a "triple equals" comparison).

Given:

```js
function hasNull(arr) {
  return true;
}
```

My answer:

```js
function hasNull(arr) {
  return arr.some(x => x === null);
}
```

Author's answer:

```js
function hasNull(arr) {
  return arr.some(x => x === null);
}
```

---

## review #1

```js
[100, 200, 300].some(n => n > 250);
// true
```

## review #2

```js
[].some(
  user => user.name === 'Betty');
// false
```

## review #3

```js
[].every(cat => cat.name === 'Ms. Fluff');
// true
```

## review #4

```js
['Amir', '', 'Betty'].every(string => string.length > 0);
// false
```

## review #5

```js
[].some(user => user.name === 'Cindy');
// false
```

## review #6

```js
[1, 2, 3].some(n => n === 2);
// true
```

## review #7

```js
[true, false, true].every(bool => bool);
// false
```

## review #8

```js
[].every(number => number > 5);
// true
```

## review #9

```js
['a', 'bc', 'def'].every(string => string.length > 0);
// true
```

## review #10

```js
[].every(user => user.name === 'Amir');
// true
```

