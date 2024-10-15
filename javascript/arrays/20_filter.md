# arrays: filter

- We often need to iterate through an array and pick out elements that meet some condition. In the next example, we have an array of users. We want to build an array of every user who is an admin. One possible solution is a for loop.

```js
const users = [
  {name: 'Amir', admin: true},
  {name: 'Betty', admin: false},
];

const admins = [];
for (let i=0; i<users.length; i++) {
  const user = users[i];
  if (user.admin) {
    admins.push(user);
  }
}

admins.map(user => user.name);
// ['Amir']
```

- That loop is easy enough to read, but we can use the built-in `.filter` method to get the same result with much less code.

- `.filter` takes a callback function and calls it with each element in an array. If the function returns `true`, then `.filter` keeps that element. Otherwise, it discards the element.

```js
const users = [
  {name: 'Amir', admin: true},
  {name: 'Betty', admin: false},
];

const admins = users.filter(
  user => user.admin
);

admins.map(user => user.name);
// ['Amir']
```

```js
const arrays = [
  [1, 2],
  [2, 3],
  [3, 4],
];
arrays.filter(array => array.includes(2));
// [[1, 2], [2, 3]]
```

- `.filter` builds and returns a new array. The original array isn't changed.

```js
const users = [
  {name: 'Amir', admin: true},
  {name: 'Betty', admin: false},
];
users.filter(user => user.admin);
users.map(user => user.name);
// ['Amir', 'Betty']
```

- JavaScript provides `.filter` for us, but it's simple to implement. We could use a `for` loop to iterate through the array, passing each element into the callback function.

```js
function filter(array, callback) {
  const results = [];
  for (let i=0; i<array.length; i++) {
    if (callback(array[i])) {
      results.push(array[i]);
    }
  }
  return results;
}
filter([1, 2, 3], x => x !== 2);
// [1, 3]
```

---

## quiz: "even numbers"

Use `.filter` to write a function that returns only the even numbers from an array.

This will require a modulo operator, `%`. You can think about `x % y` as "the remainder when dividing x by y." For example, `1 % 2 === 1`, `2 % 2 === 0`, and `3 % 2 === 1`.

`x % 2 === 0` means that a number is even.

Given:

```js
function evens(arr) {
  return [2, 4];
}
```

My answer:

```js
function evens(arr) {
  return arr.filter(i => i % 2 === 0);
}
```

Author's answer:

```js
function evens(arr) {
  return arr.filter(x => x % 2 === 0);
}
```

## quiz: "implement filter with foreach"

Use `.forEach` to write a function that behaves like `.filter`. It should take an array and a callback function. It should return a new array of only elements for which the callback is `true`.

Given:

```js
function filter(arr, callback) {
  return arr;
}
```

My answer:

```js
function filter(arr, callback) {
  const newArr = [];
  arr.forEach(x => {
    if (callback(x)) {
      newArr.push(x);
    }
  });
  return newArr;
}
```

Author's answer:

```js
function filter(arr, callback) {
  const result = [];
  arr.forEach(e => {
    if (callback(e)) {
      result.push(e);
    }
  });
  return result;
}
```

## quiz: "implement compact"

Use `.filter` to write a function `compact` that removes all `null` values from an array. It should return an array of all non-null values from the original array.

Watch out for the difference between `==` and `===`. `==` is "double equals", a loose comparison. `===` is "triple equals", a strict comparison.

Given:

```js
function compact(arr) {
  return [];
}
```

My answer:

```js
function compact(arr) {
  return arr.filter(x => !(x === null));
}
```

Author's answer:

```js
function compact(arr) {
  return arr.filter(x => x !== null);
}
```

---

## review

```js
const arrays = [
  ['Amir', 'Betty'],
  ['Cindy', 'Dalili'],
];
arrays.filter(array => array.includes('Dalili'));
// [['Cindy', 'Dalili']]
```

```js
const cats = [
  {name: 'Ms. Fluff', vaccinated: true},
  {name: 'Keanu', vaccinated: false},
];
cats.filter(cat => cat.vaccinated);
cats.map(cat => cat.name);
// ['Ms. Fluff', 'Keanu']
```

```js
const arrays = [
  [1000, 2000],
  [3000, 4000],
];
arrays.filter(array => array.includes(4000));
// [[3000, 4000]]
```
