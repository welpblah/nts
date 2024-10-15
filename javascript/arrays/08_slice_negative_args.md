# slice with negative arguments

- We've seen that `.slice` lets us get a range of elements from an array. If we pass a negative index to `.slice`, it means "give me this many elements from the end of the array". For example, `-2` means "give me the last two elements".

```js
[10, 20, 30, 40, 50].slice(-2);
// [40, 50]
```

```js
[10, 20].slice(-2);
// [10, 20]
```

- In an earlier lesson, we saw what happens when we slice past the end of the array with a positive argument. Here's a reminder: in the next example, there's no element at index 4, so we get an empty array.

```js
[10, 20].slice(4, 5);
// []
```

- With negative indexes, we can also slice before the beginning of the array. The result will only include elements in the original array. It won't invent additional elements to satisfy our out-of-bounds index.

```js
[10, 20].slice(-100);
// [10, 20]
```

- Both `start` and `end` can be negative. Remember that the `end` element isn't included in the slice.

```js
[10, 20, 30].slice(-3, -1);
// [10, 20]
```

```js
[10, 20, 30, 40, 50].slice(-3, -1);
// [30, 40]
```

```js
[10, 20, 30, 40, 50].slice(1, -1);
// [20, 30, 40]
```

```js
[10, 20, 30, 40, 50].slice(-3, 4);
// [30, 40]
```

## review #1

```js
['a', 'b', 'c'].slice(-5);
// ['a', 'b', 'c']
```

## review #2

```js
['Amir', 'Betty', 'Cindy', 'Dalili', 'Ebony'].slice(-4, -2);
// ['Betty', 'Cindy']
```

## review #3

```js
[false].slice(-1000000);
// [false]
```

## review #4

```js
[true, false, true, false, true].slice(-2, -1)
// [false]
```

## review #5

```js
[10, 20, 30, 40, 50].slice(-3, -1);
// [30, 40]
```

## review #6

```js
const arr = ['a', 'b', 'c'];
arr[-1] = 'd';
arr[-1];
// 'd'
```