# arrays: reduce

- Sometimes we want to repeatedly apply a function to each element in a list. For example, we might want to sum a list of numbers. This is simple if we know how many numbers there will be, and there are only a few of them.

```js
function add(a, b) {
  return a + b;
}
add(1, 20);
// 21
add(add(1, 20), 300);
// 321
// We can use a loop to apply this function to every number in an array.
const nums = [1, 20, 300, 4000];
let sum = 0;
nums.forEach(n => {
  sum = add(sum, n);
});
sum;
// 4321
```

- The built-in `.reduce` method simplifies this by running a function on each array element, keeping track of the result. It "reduces" the array to one return value. In this case, that value is the sum of its elements.

- We'll see `.reduce` in action first, then discuss how it works. The next example uses `.reduce` to sum an array of numbers, just like we did above.

```js
[1, 20, 300, 4000].reduce(
  (sum, current) => sum + current,
  0
);
// 4321
```

- We pass two arguments to `.reduce`. The first is a function that adds a new number to our running sum, returning the new sum. The second argument is 0, the initial value for the sum.

- The callback function is called once for each number in the array. During each call, it adds the current number to the running sum, then returns that new sum. We can see this in action by instrumenting our function to store all of the sums. (The verb "instrument" means "attach measurement instruments to". It's a great way to learn how unfamiliar systems work!)

```js
const intermediateSums = [];

[1, 20, 300].reduce(
  (sum, current) => {
    sum = sum + current;
    intermediateSums.push(sum);
    return sum;
  },
  0
);

intermediateSums;
// [1, 21, 321]
```

```js
const intermediateSums = [];

[1, 20, 300, 4000].reduce(
  (sum, current) => {
    sum = sum + current;
    intermediateSums.push(sum);
    return sum;
  },
  0
);

intermediateSums;
// [1, 21, 321, 4321]
```

- We can make our `.reduce` call shorter by omitting the second argument. In that case, the first array element is used as the sum. This works for many use cases of `.reduce`, including computing sums.

```js
[1, 20, 300].reduce((sum, current) => sum + current);
// 321
```

- When we omit the second argument, our function is never called for array element 0. To sum `[1, 20, 300]`, this happens:

- `sum` is set to element 0 of the array, which is `1`.
- `callback(1, 20)` is called, returning 21 as the new `sum`.
- `callback(21, 300)` is called, returning 321 as the new `sum`.
- There are no more array elements, so `.reduce` returns 321.

```js
const intermediateSums = [];

[1, 20, 300, 4000].reduce(
  (sum, current) => {
    sum = sum + current;
    intermediateSums.push(sum);
    return sum;
  }
);

intermediateSums;
// [21, 321, 4321]
```

- `.reduce` isn't limited to numbers. In the next example, we have users with names and numerical `points` used to score a game. We reduce the users array to the sum of points for all users.

- The second argument to the callback function still refers to the current array element. Now this element is a user object, so we name it `user` instead of `current`.

```js
const users = [
  {name: 'Amir', points: 8},
  {name: 'Betty', points: 12},
  {name: 'Cindy', points: 4},
];

const pointTotal = users.reduce((sum, user) => {
  return sum + user.points;
}, 0);

pointTotal;
// 24
```

- For that example to work, we had to provide the initial value of `0`. If we omit the initial value argument, `.reduce` will try to use the user object at index 0 as the initial value.

- Adding an object to a number causes the object to be converted into a string, `'[object Object]'`, which isn't what we want. Things only get worse from there.

```js
const users = [
  {name: 'Amir', points: 8},
  {name: 'Betty', points: 12},
  {name: 'Cindy', points: 4},
];

const pointTotal = users.reduce((sum, user) => {
  return sum + user.points;
});

pointTotal;
// '[object Object]124'
```

- Here's a closer view of what happened to produce that string:

```js
const user = {name: 'Amir', points: 8};
user + 12 + 4;
// '[object Object]124'
```

- We can use `.reduce` to emulate the behavior of `.join`, which works on strings.

```js
['x', 'y', 'z'].join('a');
// 'xayaz'
```

```js
['x', 'y', 'z'].reduce((joined, current) => joined + 'a' + current);
// 'xayaz'
```

- (The first argument of our callback function here is `joined`, not `sum`. It would be strange to call it "sum", since we're no longer adding.)

- We can write our own general-purpose `.join` using `.reduce`. It adds the separator we provide before each element. Note that we skip the second argument to `.reduce`, so that `separator` isn't added before the first element

```js
function join(array, separator) {
  return array.reduce(
    (joined, current) => joined + separator + current);
}
join(['x', 'y', 'z'], '_');
// 'x_y_z'
```

- So far, we've named the callback's first argument `sum` and `joined`. From now on, we name the callback's first argument `acc`. `acc` stands for "accumulator", because it's accumulating the result. Sometimes there are obvious better names, like `sum` or `joined`. But for some examples, there's no obvious good name.

- If one of the next few examples gives you trouble, try writing it out as a table. At each step in the `.reduce`, what are the values of `acc` and `current`? Remember that `.reduce` does little more than call the provided function over and over.

```js
[1, 200, 30].reduce((acc, current) => Math.max(acc, current));
// 200
```

```js
[1, 200, 30, 4000].reduce((acc, current) => Math.max(acc, current));
// 400
```

```js
[true, false, true].reduce((acc, current) => acc && current);
// false
```

```js
[true, false, true].reduce((acc, current) => acc || current);
// true
```

```js
[false, false, false].reduce((acc, current) => acc || current);
// false
```

```js
[[1], [2, 3], [4]].reduce((acc, current) => acc.concat(current));
// [1, 2, 3, 4]
```

- What happens when we try to reduce an empty array? If we provide an initial value as the second argument to `.reduce`, then `.reduce` will simply return that value. If the initial value is `0`, we'll get `0` out.

```js
[].reduce(
  (sum, current) => sum + current,
  0
);
// 0
```

- If we don't provide an initial value, `.reduce` has no data to work with, so it errors.

```js
[].reduce(
  (sum, current) => sum + current,
);
// TypeError: Reduce of empty array with no initial value
```

- When you're using `.reduce`, it's important to think about what happens when the array is empty. Otherwise, you may find yourself surprised if you see the error shown above.

- Reduce is a very abstract method: it can do many different things. The ways to use abstract methods aren't always obvious at first. Once you're comfortable with them, applications will usually show up.

- However, it's easy to take `.reduce` too far, so it should be used cautiously. Don't be afraid to use `.reduce` in simple cases, like summing an array of numbers. If you find yourself struggling to read or write a complex `.reduce` callback, consider using a loop instead. Saving lines is nice when it helps readability, but six easy lines of loop code are better than one confusing `.reduce` line!

---

## quiz: "all true"

Use `.reduce` to write a function that returns `true` if all array elements are `true`.

(Mind the difference between JavaScript's `==` and `===`.)

My answer:

```js
function allTrue(values) {
  return values.reduce((boolean, current) => boolean && current, true);
}
```

Author's answer:

```js
function allTrue(values) {
  /* Technically, this solution will accept arrays as long as all of
   * their values are "truthy". That includes `true`, but it also
   * includes non-zero numbers, non-empty strings, and some other values.
   */
  return values.reduce((acc, x) => acc && x, true);
}
```

## quiz: "implement filter with reduce"

Use `.reduce` to write a function that behaves like `.filter`. You can do this with only one `.reduce` and no other loops.

Three tips:

- Start with an initial value of `[]` by passing it in as `.reduce`'s second argument.
- The `acc` is the filtered array. You'll push to it when the callback returns `true` for the current element.
- Remember to return `acc` inside your reduce callback.

Given:

```js
function filter(arr, callback) {
  return arr.reduce(
    (acc, current) => {
```

My answer:

```js
function filter(arr, callback) {
  return arr.reduce(
    (acc, current) => {
      if (callback(current)) {
        acc.push(current);
      }
      return acc;
    },
    []
  );
}
```

Author's answer:

```js
function filter(arr, callback) {
  return arr.reduce(
    (acc, current) => {
      if (callback(current)) {
        return acc.concat(current);
      } else {
        return acc;
      }
    },
    []
  );
}
```

---

## review

```js
[1, 2, 3, 4].reduce(
  (sum, current) => sum + current,
  0
);
// 10
```

```js
['a', 'b', 'c'].reduce(
  (joined, current) => joined + '_' + current
);
// 'a_b_c'
```

```js
[500, 1000, 1].reduce(
  (acc, current) => Math.min(acc, current)
);
// 1
```
