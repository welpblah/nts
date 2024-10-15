# arrays: flat flatmap

- Sometimes, we have data in nested arrays and want to "flatten" these arrays into a single array. For example `[[1,2], [3, 4]]` flattens to `[1, 2, 3, 4]`.

- We can do this ourselves by looping over each element in each subarray.

```js
function flatten(arrays) {
  const flattened = [];
  arrays.forEach(array => {
    array.forEach(element => {
      flattened.push(element);
    });
  });
  return flattened;
}
flatten([
  [1, 2],
  [3, 4]
]);
// [1, 2, 3, 4]
```

- This solution works, but it's a lot of code to write for such a simple operation. Fortunately, JavaScript already provides this functionality, so we don't have to implement it ourselves. It's an array method called `.flat`.

```js
[
  [1, 2],
  [3, 4]
].flat();
// [1, 2, 3, 4]
```

- By default, `.flat` only flattens one level of arrays, just like our `flatten` function above. Any arrays nested more deeply than one level are left alone.

```js
[
  [1, 2],
  [3, [4]]
].flat();
// [1, 2, 3, [4]]
```

```js
[
  [1, 2],
  [3, [4], [[5]]],
].flat();
// [1, 2, 3, [4]. [[5]]]
```

- If we want to flatten beyond the first level, we can provide an optional `depth` argument to `.flat`. The examples above used the default value, `1`.

```js
[
  [1, 2],
  [3, [4]]
].flat(1);
// [1, 2, 3, [4]]
```

```js
[
  [1, 2],
  [3, [4]]
].flat(2);
// [1, 2, 3, 4]
```

```js
[
  [1, 2],
  [3, [[4]]]
].flat(2);
// [1, 2, 3, [4]]
```

- We don't always know how deeply-nested the arrays will be. Moreover, array elements aren't always consistent. Some may be more deeply nested than others. Fortunately, the depth can be `Infinity`, in which case `.flat` will flatten all arrays no matter how deeply nested they are.

```js
[
  [1, 2],
  [3, [[4]]]
].flat(Infinity);
// [1, 2, 3, 4]
```

```js
const array = [[[[[[[[[[[[[[1]]]]]]]]]]]]]];
// [1]
```

- `.flat` only flattens arrays. Non-array values are left unmodified. For example, we might have an array of objects, where the objects contain more arrays. In that case, both the objects and the arrays inside the objects are left unchanged.

```js
[
  [1, 2],
  {name: 'Amir', postIds: [77, 192]},
].flat(Infinity);
// [1, 2, { name: 'Amir', postIds: [77, 192]}]
```

Suppose that we're working on a system where each user can have multiple email addresses. We want to build a list of every email address for every user.

There's a nice solution in two steps. First, we build an array of the users' email addresses. Each user has an array of email addresses, so the overall array will be nested one level deep.

```js
const users = [
  {name: 'Amir', emails: ['amir@example.com', 'amir2@example.com']},
  {name: 'Betty', emails: ['betty@example.com']}
];
users.map(user => user.emails);
// [['amir@example.com', 'amir2@example.com'], ['betty@example.com']]
```

Next, we flatten this array. Since `.map` returns a new array, we can chain `.flat` directly after `.map`.

#### code problem (1/2)

Modify the code below to flatten the email array with `.flat`. That will give us a single flat array of every user's email address.

Given:

```js
const users = [
  {name: 'Amir', emails: ['amir@example.com', 'amir2@example.com']},
  {name: 'Betty', emails: ['betty@example.com']}
];

users.map(user => user.emails);
```

My answer: 

```js
const users = [
  {name: 'Amir', emails: ['amir@example.com', 'amir2@example.com']},
  {name: 'Betty', emails: ['betty@example.com']}
];

users.map(user => user.emails).flat();
```

Author's answer:

```js
const users = [
  {name: 'Amir', emails: ['amir@example.com', 'amir2@example.com']},
  {name: 'Betty', emails: ['betty@example.com']}
];

users.map(user => user.emails).flat();
```

- Combining `.flat` with `.map` is common, so there's a built in method to do both: `.flatMap`. It's identical to calling `.map` followed by `.flat` (with the default depth of 1.)

```js
[
  {numbers: [1, 2]},
  {numbers: [3, 4]},
].map(obj => obj.numbers);
// [[1, 2], [3, 4]]
```

```js
[
  {numbers: [1, 2]},
  {numbers: [3, 4]},
].map(obj => obj.numbers).flat();
// [1, 2, 3, 4]
```

```js
[
  {numbers: [1, 2]},
  {numbers: [3, 4]},
].flatMap(obj => obj.numbers);
// [1, 2, 3, 4]
```

#### code problem (2/2)

Use `.flatMap` to get a flat array of all of the users' email addresses.

Given:

```js
const users = [
  {name: 'Amir', emails: ['amir@example.com', 'amir2@example.com']},
  {name: 'Betty', emails: ['betty@example.com']}
];
```

My answer:

```js
const users = [
  {name: 'Amir', emails: ['amir@example.com', 'amir2@example.com']},
  {name: 'Betty', emails: ['betty@example.com']}
];

users.flatMap(user => user.emails);

// ['amir@example.com', 'amir2@example.com', 'betty@example.com']
```

Author's answer: 

```js
const users = [
  {name: 'Amir', emails: ['amir@example.com', 'amir2@example.com']},
  {name: 'Betty', emails: ['betty@example.com']}
];

users.flatMap(user => user.emails);

// ['amir@example.com', 'amir2@example.com', 'betty@example.com']
```

- Both `.flat` and `.flatMap` build and return a new array. The original array remains unmodified.

```js
const numbers = [
  1,
  [2, 3]
];
numbers.flat();
numbers;
// [1, [2, 3]]
```

### review

```js
[
  [1, 2],
  {name: 'Amir', postIds: [77, 192]},
].flat(Infinity);
// [1, 2, {name: 'Amir', postIds: [77, 192]}]
```

### review

Use `.flatMap` to get a flat array of all of the users' email addresses.

Given:

```js
const users = [
  {name: 'Amir', emails: ['amir@example.com', 'amir2@example.com']},
  {name: 'Betty', emails: ['betty@example.com']}
];
```

My answer:

```js
const users = [
  {name: 'Amir', emails: ['amir@example.com', 'amir2@example.com']},
  {name: 'Betty', emails: ['betty@example.com']}
];

users.flatMap(user => user.emails);

// ['amir@example.com', 'amir2@example.com', 'betty@example.com']
```

---
```js
const numbers = ['a', ['b', 'c']];
numbers.flat();
numbers;
// ['a', ['b', 'c']]
```

```js
[
  [1, 2],
  [3, [[4]]]
].flat(2);
// [1, 2, 3, [4]]
```

```js
const numbers = [
  1,
  [2, 3]
];
numbers.flat();
numbers;
// [1, [2, 3]]
```
---
Use `.flatMap` to get a flat array of all of the users' email addresses.

```js
const users = [
  {name: 'Amir', emails: ['amir@example.com', 'amir2@example.com']},
  {name: 'Betty', emails: ['betty@example.com']}
];

users.flatMap(user => user.emails);
// ['amir@example.com', 'amir2@example.com', 'betty@example.com']
```