# includes

- We can check for whether an array includes a given element.

```js
['a', 'b'].includes('c');
// false
```

```js
['a', 'b'].includes('a');
// true
```

- Some methods are mercifully simple to learn.

## review #1

```js
[true].includes(false);
// false
```

## review #2

```js
[true].includes(true);
// true
```

## review #3

```js
[].includes(0);
// false
```

## review #4

```js
['Amir', 'Betty', 'Cindy'].includes('Betty');
// true
```

## review #5

```js
['a', 'b'].includes('a');
// true
```

### quiz: "implement uniq"

Write a function `uniq(arr)`. It should return a new array without any duplicate values. It shouldn't change the original array.

Given: 

```js

```

My answer:

```js
function uniq(arr) {
  /* Build and return a new array here. */
  let newArray = [];
  arr.forEach(element => {
    if (!(newArray.includes(element))) {
      newArray.push(element);
    }
  });
  return newArray;
}
```

Author's answer:

```js
function uniq(arr) {
  const newArr = [];
  arr.forEach(e => {
    if (!newArr.includes(e)) {
      newArr.push(e);
    }
  });
  return newArr;
}
```