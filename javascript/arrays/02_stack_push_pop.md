# arrays: stack push pop

- We can add elements to the end of an array with `.push(element)`.

```js
const numbers = [1, 2];
numbers.push(3);
numbers;
// [1, 2, 3]
```

- `.push` returns the array's length, including the newly-added element.

```js
const strings = ['a', 'b'];
strings.push('c');
// 3
```

```js
const strings = ['a', 'b', 'c', 'd'];
strings.push('e');
// 5
```

- Unlike some other array methods, `.push` doesn't return the array. It changes ("mutates") the array each time it's called.

```js
const numbers = [1];
numbers.push(2);
numbers;
// [1, 2]
```

- `.pop` is the opposite of push. It removes the last element from the array.

```js
const numbers = [1, 2, 3];
numbers.pop();
numbers;
// [1, 2]
```

- `.pop` returns the element that was removed.

```js
const numbers = [1, 2, 3];
numbers.pop();
// 3
```

- If the array is empty, `.pop` returns `undefined`, because there's nothing to remove. This mirrors the way that array indexing works: if we ask for any index of an empty array, we get `undefined`.

```js
const arr = [];
arr[0];
// undefined
```

```js
const arr = [];
arr.pop();
// undefined
```

- Like `.push`, `.pop` also mutates the array.

```js
const numbers = [1, 2, 3];
numbers.pop();
numbers.pop();
numbers;
// [1]
```

```js
const a = [1, 2, 3];
a.pop();
a.push('a');
a;
// [1, 2, 'a']
```

---

## review

```js
const arr = [true];
arr.push(false);
// 2
```

```js
const arr = [false, false];
arr.push(true);
arr;
// [false, false, true]
```

```js
const names = ['Amir', 'Betty'];
names.pop();
// 'Betty'
```

```js
const names = ['Amir', 'Betty'];
names.pop();
names;
// ['Amir']
```

```js
const names = ['Amir', 'Betty'];
names.push('Cindy');
names;
// ['Amir', 'Betty', 'Cindy']
```

```js
const arr = [];
arr.push(null);
// 1
```

```js
const arr = [true, false, true, false];
arr.pop();
// false
```

```js
const arr = [true, false, true, false];
arr.pop();
arr;
// [true, false, true]
```