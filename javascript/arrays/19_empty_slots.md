# arrays: empty slots

- When we use `new Array`, the array appears to be full of `undefined`s.

```js
new Array(1)[0];
// undefined
```

- However, index 0 of that array doesn't contain `undefined`. Instead, it's an "empty slot". Empty means that there's nothing in the slot, not even an `undefined`. But JavaScript has to give us some value, so it gives us `undefined`.

- For arrays, `x in a` asks whether the array has something in index x. For example, the array `['a', 'b', 'c']` has elements in indexes 0 through 2.

```js
const numbers = ['a', 'b', 'c'];
1 in numbers;
// true
3 in numbers;
// false
```

- We can use the `in` operator to see the difference between an array containing `undefined` and an array with an empty slot. First, here's an array containing `undefined`:

```js
const array = new Array(1).fill(undefined);
array[0];
// undefined
```

```js
0 in new Array(1).fill(undefined);
// true
```

- Now here's an array with an empty slot. We still get `undefined` when we look inside of the array, but the index 0 is not in the array!

```js
const array = new Array(1);
array[0];
// undefined
```

```js
0 in new Array(1);
// false
```

- Adding elements past the end of an array causes the array to grow.

```js
const array = [];
array[2] = 1;
array.length;
// 3
```

- We never put a value in indexes 0 or 1, so those slots are empty. Accessing them gives us `undefined`.

```js
const array = [];
array[2] = 1;
array[0];
// undefined
```

- Like before, we can use the `in` operator to see that some slots are empty.

```js
const array = [];
array[2] = 1;
2 in array;
// true
```

```js
const array = [];
array[2] = 1;
1 in array;
// false
```

- We can `.fill` in those empty slots. That's why `.fill` is so often paired with `new Array()`. This is a good way to avoid arrays with empty slots.

```js
const allEmpty = new Array(3);
allEmpty.fill('d');
// ['d', 'd', 'd']
```

- Empty slots may seem like a curiosity, but they can lead to confusing bugs. For example, the `.forEach` method skips over empty slots when looping. It doesn't even call our callback function for those slots!

```js
const array = new Array(3);
let elementsCounted = 0;
array.forEach(x => {
  elementsCounted += 1;
});
elementsCounted;
// 0
```

```js
const array = new Array(3);
array[1] = true;
let elementsCounted = 0;
array.forEach(x => {
  elementsCounted += 1;
});
elementsCounted;
// 1
```

- `.map` also behaves strangely with empty slots. It calls our callback function for each non-empty slot, as usual. But like `.forEach`, it doesn't even call our callback for empty slots. They're still empty in the final array, even though it seems like `.map` should've replaced them.

```js
const allEmpty = new Array(2);
const stillEmpty = allEmpty.map(x => true);
[stillEmpty[0], stillEmpty[1]];
// [undefined, undefined]
```

```js
const allEmpty = new Array(2);
const stillEmpty = allEmpty.map(x => true);
0 in stillEmpty;
// false
```

- `.reduce`, `.filter`, and some other methods also skip empty slots. Here are our tips to avoid arrays with empty slots:

	- Always use `.fill` after calling `new Array(someSize)`.
	- Don't write to indexes past the end of an array.

- Finally, a note on terminology. An array with empty slots is sometimes called a "sparse" array. "Sparse" means "thinly dispersed or scattered", which is appropriate for an array where only some of the elements actually exist!

---

## review #1

```js
const values = new Array(10);
values[1] = 'some value';
values[2] = 'another value';

let valuesCounted = 0;
values.forEach(x => {
  valuesCounted += 1;
});
valuesCounted;
// 2
```

## review #2

```js
const array = [];
array[100] = 'some value';
1 in array;
// false
```

## review #3

```js
/* Note that this array will have a mixture of empty slots and slots
 * that contain a value. */
const values = new Array(10);
values[1] = 'some value';

const mapped = values.map(x => true);
[0 in mapped, 1 in mapped];
// [false, true]
```

## review #4

```js
0 in new Array(200);
// false
```

## review #5

```js
const array = new Array(3);
array[1] = true;
let elementsCounted = 0;
array.forEach(x => {
  elementsCounted += 1;
});
elementsCounted;
// 1
```

## review #6

```js
const array = [];
array[5] = true;
1 in array;
// false
```

## review #7

```js
const allEmpty = new Array(2);
const stillEmpty = allEmpty.map(x => true);
0 in stillEmpty;
// false
```

## review #8

```js
const array = [];
array[2] = 1;
1 in array;
// false
```