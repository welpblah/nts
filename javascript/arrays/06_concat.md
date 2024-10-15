# concat

- In many languages, `+` combines two arrays. That's not the case in JavaScript. Trying to do `array1 + array2` is usually a mistake. JavaScript will convert the arrays into strings before adding them.

```js
[1, 2].toString();
// '1,2'
```

```javascript
[1, 2] + [3];
// '1,23'
```

```js
[1, 2] + [3, 4];
// '1,23,4'
```

- However, we can combine arrays properly with `.concat`. (It's short for "concatenate", which means "link together".) This creates a new array containing all of the elements from the original arrays.

```js
[1, 2].concat([3, 4]);
// [1, 2, 3, 4]
```

- `.concat` takes multiple arguments, so we can combine many arrays if needed:

```js
[1, 2].concat([3, 4], [5, 6]);
// [1, 2, 3, 4, 5, 6]
```

- If we pass a single element to `.concat`, it'll add it to the end of an array.

```js
[1, 2].concat(3);
// [1, 2, 3]
```

- At first, using `.concat` on a single element looks like using `.push`. But there's a crucial difference: `.push` mutates the array we're pushing to.

```js
const oldNumbers = [1, 2];
oldNumbers.push(3);
oldNumbers;
// [1, 2, 3]
```

- `.concat` builds and returns a new array. The original arrays aren't changed.

```js
const numbers1 = [1, 2];
const numbers2 = [3, 4];
numbers1.concat(numbers2);
numbers1;
// [1, 2]
```

```js
const oldNumbers = [1, 2];
const newNumbers = oldNumbers.concat(3);
oldNumbers;
// [1, 2]
```

### review

```js
[
  [
    1,
    [
      2,
    ],
  ],
  {name: 'Ms. Fluff', vaccinated: true},
].flat(Infinity);
// [1, 2, {name: 'Ms. Fluff', vaccinated: true}]
```

```js
[
  1,
  [2, [3, [4]]],
].flat(2);
// [1, 2, 3, [4]]
```

```js
['a', 'b'].concat(['c'], ['d']);
// ['a', 'b', 'c', 'd']
```

```js
const letters1 = ['a', 'b'];
const letters2 = ['c', 'd'];
letters1.concat(letters2);
letters1;
// ['a', 'b']
```

```js
[true, false].concat([true], [false, true]);
// [true, false, true, false, true]
```

```js
const booleans1 = [true];
const booleans2 = [false];
booleans1.concat(booleans2);
booleans1;
// [true]
```