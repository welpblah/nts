# arrays: negative array indexes

- In another lesson, we saw that `.slice` can accept negative arguments.

```js
['a', 'b', 'c'].slice(-1);
// ['c']
```

- We might expect that to work with regular array indexes, like `someArray[-1]`. Unfortunately, it doesn't work. Instead, it returns `undefined`, the same value that we get when accessing any index that doesn't exist.

```js
const arr = ['a', 'b', 'c'];
arr[-1];
// undefined
```

- What if we try to store a value in a negative array index? Surprisingly, this does work, and we're be able to read the value back!

```js
const arr = ['a', 'b', 'c'];
arr[-1] = 'd';
arr[-1];
// 'd'
```

- We might assume we're assigning a value at the -1 index. However, what's actually happening is more subtle.

- In another lesson, we saw that arrays are a special kind of object, and can have normal object properties.

```js
const arr = ['a', 'b', 'c'];
arr['name'] = 'Bob';
arr['name'];
// 'Bob'
```

- That's what's happening here. Since negative indexes aren't allowed, our `-1` is being converted into the string `'-1'` and used as an object property.

```js
const arr = ['a', 'b', 'c'];
arr[-1] = 'd';
Object.keys(arr);
// ['0', '1', '2', '-1']
```

- If we loop over the array with `.forEach`, we won't see the "negative indexes" because they're not actually array indexes. This is consistent with other custom properties that we add to arrays.

```js
const arr1 = ['a', 'b', 'c'];
const arr2 = [];
arr1[-1] = 4;
arr1.forEach(i => arr2.push(i));
arr2;
// ['a', 'b', 'c']
```

This "negative index" behavior can cause some very subtle bugs! Here's an example.

The function below returns the first array element greater than 2. If no element is greater than 2, it returns `undefined`.

```js
function findGreaterThan2(arr) {
  const index = arr.findIndex(x => x > 2);
  return arr[index];
}
// undefined
findGreaterThan2([1, 2, 3]);
// 3
findGreaterThan2([0, 1]);
// undefined
findGreaterThan2([1, 2]);
// undefined
const arr = [1, 2];
arr[-1] = 6;
findGreaterThan2(arr);
// 6
```

Our function uses `.findIndex`, which returns -1 when it can't find a matching element. When no matching element is found, the `arr[index]` lookup becomes `arr[-1]`. That returns `undefined` because the array has no `'-1'` property. Our `findGreaterThan2` function then returns that `undefined`, as expected.

But what if we call our function on an array with a `'-1'` property?

Like before, the `.findIndex` call still returns `-1`, so `arr[index]` still becomes `arr[-1]`. And like before, that expression looks up an object property on the array, as if we'd done `arr['-1']`. If the array actually does have a property named `'-1'`, then the value at that property will be returned!

Since no element in the array is greater than 2, our function should've returned `undefined`. Instead, it returned `6`, even though the array only contains `[1, 2]`!

Be careful: many programming languages do allow negative indexes, but JavaScript doesn't. It won't throw an exception if you try to read from or write to negative indexes. Instead, you'll get confusing results like those above!

---

## review

```js
const arr = [true, false];
arr[-3];
// undefined
```

```js
const arr = [100, 200];
arr[-1] = 300;
arr[-1];
// 300
```

```js
const arr1 = [100, 200];
const arr2 = [];
arr1[-1] = 300;
arr1.forEach(i => arr2.push(i));
arr2;
// [100, 200]
```

```js
const arr = ['one', 'million'];
arr[-1000000];
// undefined
```

```js
const arr = [true, true];
arr[-1] = false;
arr[-1];
// false
```

```js
const arr1 = [true, true];
const arr2 = [];
arr1[-1] = false;
arr1.forEach(i => arr2.push(i));
arr2;
// [true, true]
```
