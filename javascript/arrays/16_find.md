# arrays: find

- Sometimes we need to find an element that matches a condition. We already saw `.findIndex`, which returns the index of a matching element. Now let's write our own `.find` function that returns the matching element itself.

- Our function first get the index of the matching element with `.findIndex`. If the index is `-1`, the element wasn't found, so we return `undefined`. Otherwise, we return the element at that index.

```js
function find(array, callback) {
  const index = array.findIndex(callback);
  if (index === -1) {
    return undefined;
  } else {
    return array[index];
  }
}
find([5, 6, 7], n => n / 2 === 3);
// 6
```

- Fortunately, JavaScript arrays have a built-in `.find` method, so we don't have to write it ourselves.

```js
[5, 6, 7].find(n => n / 2 === 3);
// 6
```

- We can use this to find objects with certain properties.

```js
const users = [
  {name: 'Amir', admin: false},
  {name: 'Betty', admin: true},
];
users.find(user => user.admin).name;
// 'Betty'
```

- If there are multiple matches, `.find` will return the first matching element. This is the same behavior we saw with `.indexOf` and `.findIndex`.

In the next example, we use `.find` to match every even number in the array. (The modulo operator `%` returns the remainder of a division. `n % 2 === 0` is true when `n` is even.)

```js
const numbers = [1, 2, 3, 4, 5, 6];
numbers.find(n => n % 2 === 0);
// 2
```

- Sometimes we want to find multiple elements that match a condition. The `find` method can't do that, but the `filter` method can. We'll see `filter` in another lesson.

---

## review #1

```js
['Amir', 'Betty', 'Cindy'].find(name => name.length === 4);
// 'Amir'
```

## review #2

```js
const cats = [
  {name: 'Ms. Fluff', vaccinated: true},
  {name: 'Keanu', vaccinated: false},
];
cats.find(cat => !cat.vaccinated).name;
// 'Keanu'
```

## review #3

```js
[
  [],
  [1],
  [1, 2],
].find(array => array.length === 1);
// [1]
```

## review #4

```js
const users = [
  {name: 'Amir', admin: false},
  {name: 'Betty', admin: true},
];
users.find(user => user.admin).name;
// 'Betty'
```

## review #5

```js
const users = [
  {name: 'Amir', admin: false},
  {name: 'Betty', admin: true},
];
users.find(user => user.admin).name;
// 'Betty'
```
