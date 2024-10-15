# arrays: reverse

- We can reverse arrays with the `.reverse` method. This works as we'd expect.

```js
const a = [3, 2, 1];
a.reverse();
// [1, 2, 3]
```

- This is sometimes useful with sorting. The next example sorts strings in alphabetical order.

```js
const strings = ['k', 'o', 'a', 'l', 'p'];
strings.sort().join('');
// 'aklop'
```

- We might want to sort the strings in reverse-alphabetical order instead. No need to write a custom comparison function; we can just sort and reverse.

```js
const strings = ['k', 'o', 'a', 'l', 'p'];
strings.sort();
strings.reverse().join('');
// 'polka'
```

- `.reverse` modifies the array it's called on, like `.sort` does.

```js
const numbers = [1, 2, 3];
numbers.reverse();
numbers;
// [3, 2, 1]
```

```js
const strings = ['k', 'o', 'a', 'l', 'p'];
strings.sort();
strings.reverse();
strings.join('');
// 'polka'
```

---

## review #1

```js
const a = ['Amir', 'Betty'];
a.reverse();
// ['Betty', 'Amir']
```

## review #2

```js
const strings = ['Amir', 'Betty'];
strings.reverse();
strings;
// ['Betty', 'Amir']
```

## review #3

```js
const a = [true, false];
a.reverse();
// [false, true]
```

## review #4

```js
const numbers = [100, 200, 300];
numbers.reverse();
numbers;
// [300, 200, 100]
```

## review #5

```js
const strings = ['k', 'o', 'a', 'l', 'p'];
strings.sort();
strings.reverse();
strings.join('');
// 'polka'
```