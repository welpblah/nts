# arrays: map

- We often want to build a new array out of an existing array. For example, we might want to convert an array of dollar prices into cents, multiplying each by 100.

- `.map` is good for jobs like this. We give `.map` a callback, which `.map` calls on each array element. `.map` returns a new array of the values returned by those function calls.

```js
const dollarAmounts = [10, 20, 30];
const centAmounts = dollarAmounts.map(n => n * 100);
centAmounts;
// [1000, 2000, 3000]
```

- We can use `.map` to uppercase each string in an array.

```js
['a', 'b', 'c'].map(x => x.toUpperCase());
// ['A', 'B', 'C']
```

- Note that `.map` doesn't change the original array.

```js
const nums = [1, 2, 3];
nums.map(n => n * 10);
nums[0];
// 1
```

- In a previous lesson, we built an array of people's names using `.forEach`.

```js
const people = [
  {name: 'Amir'},
  {name: 'Betty'},
];
const names = [];
people.forEach(person => {
  names.push(person.name);
});
names;
// ['Amir', 'Betty']
```

- With `.map`, we can avoid pushing each element onto a new array. Instead, we map the original array elements to create new elements in a single operation. Note how much simpler and cleaner this version looks!

```js
onst people = [
  {name: 'Amir'},
  {name: 'Betty'},
];
const names = people.map(person => person.name);
names;
// ['Amir', 'Betty']
```

- When repeated hundreds or thousands of times across a software system, this difference can really add up.

- Like `.forEach`, the callback function we pass to `.map` takes an optional second argument: the element's index. This can be useful when we need to keep track of element count, or only operate on every nth element.

```js
const people = [
  {name: 'Amir'},
  {name: 'Betty'},
];
people.map((person, i) => {
  return (i + 1) + ': ' + person.name;
});
// ['1: Amir', '2: Betty']
```

```js
const numbers = [10, 10, 10];
numbers.map((e, i) => e * i);
// [0, 10, 20]
```

- In JavaScript, any function without a `return` statement implicitly returns `undefined`.

```js
function double(n) {
  const doubled = n * 2;
}
double(3);
// undefined
```

- If we forget to `return` an element in the callback function, the same thing happens: our callback returns `undefined`. The `undefined`s will end up in our mapped array. This is an easy mistake to make.

```js
[1,2].map(n => {
  n * 2;
});
// [undefined, undefined]
```

- One-line arrow functions (without curly braces) return their values automatically, so that's one way to avoid the problem.

```js
[1, 2, 3].map(n => n * 2);
// [2, 4, 6]
```

---

## code problem 1/2

The `.map` below should return an array of doubled numbers. Instead, it returns an array of `undefined`s. Modify it to correctly double each number.

Given:

```js
[1,2,3].map(n => {
  n * 2;
});
```

My answer:

```js
[1,2,3].map(n => n * 2);
// [2, 4, 6]
```

Author's answer:

```js
[1,2,3].map(n => n * 2);
// // [2, 4, 6]
```

- `.map` can make code easier to read because it doesn't mutate the original array. We don't have to ask "is this modifying an array that's visible to other code in the system?" When you're tempted to reach for a `for` loop, `for ... of` loop, or the `.forEach` method, consider whether `.map` might be a better solution.

---

## quiz: "add exclamation"

Use `.map` to write a function that appends a '!' to every element.

Given: 

```js
function excite(strings) {
  return strings;
}
```

My answer:

```js
function excite(strings) {
  return strings.map(str => str + '!'); 
}
```

Author's answer:

```js
function excite(strings) {
  return strings.map((c) => c + '!');
}
```

## quiz: "square"

Use `.map` to write a function that squares every number in an array.

Given:

```js
function square(nums) {
  return nums;
}
```

My answer:

```js
function square(nums) {
  return nums.map(n => n ** 2);
}
```

Author's answer:

```js
function square(nums) {
  return nums.map(n => n * n);
}
```

---

## review

```js
[1, 2, 3].map(n => n * 100);
/// [100, 200, 300]
```

```js
const nums = [10, 20, 30];
nums.map(n => n * 10);
nums[1];
// 20
```

```js
[1, 2, 3].map(n => n + 5);
// [6, 7, 8]
```

```js
const nums = [5, 6, 7];
nums.map(n => n * 10);
nums[2];
// 7
```