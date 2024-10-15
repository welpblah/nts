# arrays: sort

- The `.sort` method puts elements in order. We can use it to sort an array of strings alphabetically.

```js
const strings = ['c', 'a', 'b'];
strings.sort();
strings;
// ['a', 'b', 'c']
```

- `.sort` modifies the array. We can see that in the example above: we sorted the array, then looked at it, and its content had changed. However, `.sort` also returns the array to us.

```js
const strings = ['c', 'a', 'b'];
strings.sort();
// ['a', 'b', 'c']
```

- Often, we want a sorted copy of an array without changing the original. We can use `.slice` to copy the array, then sort the copy.

```js
const originalStrings = ['c', 'a', 'b'];
const sortedStrings = originalStrings.slice().sort();
sortedStrings;
// ['a', 'b', 'c']
originalStrings;
// ['c', 'a', 'b']
```

- JavaScript often converts values in surprising ways. For example, `10 > 3` is true, but `'10' < '3'` is also true. This is because strings are compared character by character. The comparison ends as soon as two characters differ. `'1' < '3'`, so `'10' < '3'`. The '0' in '10' is never even examined.

```js
'3' > '10';
// true
```

```js
'200' > '3';
// false
```

- `.sort` converts the array's elements to strings, then compares them. Because it compares strings, it inherits the comparison problem above.

- This makes JavaScript's `.sort` unsafe for numbers and most other data. Most other programming languages don't have this problem, so be careful!

```js
[3, 10].sort();
// [10, 3]
```

```js
[3, 10, 200].sort();
// [10, 200, 3]
```

- There's a way to fix this problem. We can write our own comparison function, then give it to `.sort`.

- The `.sort` function will call our comparison function repeatedly. Each time, our function gets two array elements `(a, b)` as arguments and returns:

- 0 if the two elements are equal.
- A number greater than zero if `a` is greater.
- A number smaller than zero if `b` is greater.

- To start, `.sort` will call our function with the values at indexes 1 and 0 in the array. Then it will continue to pass other pairs of array elements as it determines what the sorted order should be.

- Our comparison function only needs to determine which of two elements is greater, which is a relatively simple problem. By calling our comparison function over and over again, `.sort` eventually solves the more difficult problem of sorting the entire array.

```js
[10, 200, 3].sort((a, b) => {
  if (a === b) {
    return 0;
  } else if (a > b) {
    return 1;
  } else {
    return -1;
  }
});
// [3, 10, 200]
```

- The above comparison function is very wordy. Fortunately, there's a shorthand that we can use. It relies on a trick:

- If `a === b`, then `a - b === 0`.
- If `a > b`, then `a - b > 0`.
- If `a < b`, then `a - b < 0`.

```js
3 - 10 > 0;
// false
```

```js
10 - 3 > 0;
// true
```

- Now here's the trick: our callback function can return `a - b`. According to the three cases outlined above, `a - b` will correctly return a positive number, negative number, or 0, which is exactly what `.sort` wants.

```js
[10, 200, 3].sort((a, b) => a - b);
// [3, 10, 200]
```

- We can use the comparison function for more than just numbers. For example, we can sort arrays by their lengths.

```js
[[0, 0], [], [0, 0, 0], [0]].sort((a, b) => a.length - b.length);
// [[], [0], [0, 0], [0, 0, 0]]
```

- We can even sort objects by a property.

```js
const users = [
  {name: 'Amir', loginAttempts: 5},
  {name: 'Betty', loginAttempts: 3}
];
users.sort((user1, user2) => {
  return user1.loginAttempts - user2.loginAttempts;
}).map(user => user.name);
// ['Betty', 'Amir']
```

---

## review

```js
const strings = ['c', 'a', 'b'];
strings.sort();
// ['a', 'b', 'c']
```

```js
[30, 200, 1000].sort();
// [1000, 200, 30]
```

```js
[1000, 200, 30].sort((a, b) => a - b);
// [30, 200, 1000]
```

```js
const strings = ['c', 'a', 'b'];
strings.sort();
// ['a', 'b', 'c']
```

```js
[999, 1000].sort();
// [1000, 999]
```

---

## quiz: "sort by word length"

Use `.sort` to write a `sortByLength` function. It sorts an array of strings by their length, with the shortest string first. The function should change (mutate) the array, just as `.sort` modifies the array it's called on. (Your function doesn't need to return a new array.)

My answer:

```js
function sortByLength(words) {
  return words.sort((a, b) => a.length - b.length);
}
```

Author's answer:

```js
function sortByLength(words) {
  words.sort((a, b) => a.length-b.length);
}
```
