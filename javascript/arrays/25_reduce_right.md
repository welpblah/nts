# arrays: reduce right

- `.reduce` processes array items from left to right. There's also a less-common variant called `.reduceRight`. With `.reduceRight`, the array is processed from right to left.

- This doesn't matter for many operations, like summing numbers. We get the same sum whether we do `1 + 20 + 300` or `300 + 20 + 1`.

```js
[1, 20, 300].reduce((acc, current) => acc + current);
// 321
```

```js
[1, 20, 300].reduceRight((acc, current) => acc + current);
// 321
```

- However, `.reduceRight` produces a different result when the order of operations matters. In the next example, we concatenate (join) strings in reverse order. `.reduceRight` begins at the right (`'c'`) and ends at the left (`'a'`).

```js
['a', 'b', 'c'].reduceRight((acc, current) => acc + current);
// 'cba'
```

- Left vs. right is also important for subtraction. `.reduce` and `.reduceRight` differ here because `20 - 1 != 1 - 20`.

```js
[20, 1].reduce((acc, current) => acc - current);
// 19
```

```js
[20, 1].reduceRight((acc, current) => acc - current);
// -19
```

- When the order of operations matters, remember that `.reduceRight` is available!

---

## review 

```js
['1', '2', '3'].reduceRight((acc, current) => acc + current);
// '321'
```
