# core: forof loops

JavaScript has a `for...in` loop that can iterate over object keys (not values), arrays, and strings.

```js
const obj = {
  'a': 1,
  'b': 2,
};
const keys = [];
for (const key in obj) {
  keys.push(key);
}
keys;
// ['a', 'b']
```

This seems straightforward at first, but things can get confusing in some situations. One notorious problem occurs when looping over arrays.

Unlike most other languages, JavaScript's arrays are a special kind of JavaScript object. `typeof someArray` is `'object'`, not `'array'`!

```js
typeof [1, 2, 3];
// 'object'
```

We can perform any object operation on arrays. For example, we can ask for their keys. The "keys" of `['apple', 'banana']` are `['0', '1']`: the array's indexes are converted into strings.

```js
const fruits = ['apple', 'banana', 'carrot'];
Object.keys(fruits);
// ['0', '1', '2']
```

We get the same result if we use a `for... in` loop on the array. It iterates over object keys (not values). Because arrays are objects, we once again get each of the array's indexes as a string like `'0'`.

```js
// Note: this code example reuses elements (variables, etc.) defined in earlier examples.
const elements = [];
for (const fruit in fruits) {
  elements.push(fruit);
}
elements;
// ['0', '1', '2']
```

Getting array indexes as strings is not very useful, but at least it's predictable. Next, we'll see examples where `for...in` loops break down in more confusing ways.

JavaScript arrays can be "sparse": they can have elements missing. Suppose that we create an empty array (`const numbers = []`) and insert a value at index 3 (`numbers[3] = 'a'`). In most languages (Java, Python), that's an error. Some languages (Ruby) implicitly fill indexes 0 through 2 with `null`, `nil`, or whatever other "notavalue" value the language has.

JavaScript does neither of those! That array has a value at index 3, but it has nothing at indexes 0, 1, or 2. This is an important point: the values at indexes 02 aren't `undefined` or `null`. The array doesn't have anything there at all.

This exposes one of `for...in` loops' edge cases. This loop skips missing array elements! If we loop over the array with `for...in`, the keys 02 don't even show up. Only 3 shows up (as the string `'3'`)!

```js
const letters = [];
letters[3] = 'a';
const keys = [];
for (const key in letters) {
  keys.push(key);
}
keys;
// ['3']
```

This unexpected behavior can cause very confusing bugs. When working with arrays, we write code that expects them to start at 0. In fact, "starts at index 0" is definitional for arrays in most languages, but not in JavaScript! (There are also similar problems with regular, nonarray objects, but this array problem is much quicker to demonstrate.)

Fortunately, modern JavaScript provides a better type of loop: `for...of`. It loops over the values in an array, not the keys. This mimics the "foreach" loops available in Java, Python, Ruby, and most other languages.

```js
const letters = ['a', 'b', 'c'];
const result = [];
for (const letter of letters) {
  result.push(letter);
}
result;
// ['a', 'b', 'c']
```

The `for...of` loop is versatile: we can adapt it to address the problems we ran into with the `for...in` loop.

Let's start with a sparse array (an array with some missing indexes). Here, the `for...of` loop behaves as we'd expect. It iterates from index 0 until the highest index in the array. If an index is missing, the loop gives us `undefined`.

In the example below, the loop body executes four times, giving us `undefined` the first three times.

(Remember that `for...of` returns values, not keys!)

```js
const numbers = [];
numbers[3] = 'a';
const result = [];
for (const n of numbers) {
  result.push(n);
}
result;
// [undefined, undefined, undefined, 'a']
```

What if we want to iterate over an object's keys, like `for...in` does? Fortunately, since 2011 JavaScript has had an `Object.keys` method that gives us an object's keys as an array. Then we can `for...of` over that array.

```js
const obj = {a: 1, b: 2};
const keys = [];
for (const key of Object.keys(obj)) {
  keys.push(key);
}
keys;
// ['a', 'b']
```

And what if we want to iterate over a string? Once again, `for...of` does what you'd expect! The loop body executes once per character in the string. JavaScript doesn't have a dedicated character type, so the individual characters will show up as strings of length 1, like `'a'`.

```js
const s = 'loop';
const chars = [];
for (const char of s) {
  chars.push(char);
}
chars;
// ['l', 'o', 'o', 'p']
```

`for...of` loops also work well with other modern JavaScript features like iterators and generators. We'll see concrete examples when we discuss those topics.

As is often the case, linters can stop you from accidentally using the old `for...in` syntax. ESLint's [guardforin](https://eslint.org/docs/rules/guardforin) rule will stop you from accidentally using `for...in` when you mean `for...of`. (But it will still allow `for...in` if you use certain patterns to make it safe.)

---

## review

```js
const numbers = [];
numbers[2] = 'some value';
const result = [];
for (const n of numbers) {
  result.push(n);
}
result;
// [undefined, undefined, 'some value']
```

```js
const s = 'loop';
const chars = [];
for (const char of s) {
  chars.push(char);
}
chars;
// ['l', 'o', 'o', 'p']
```