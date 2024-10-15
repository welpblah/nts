# arrays: new and fill

- The `.fill` method replaces all of an array's existing values with one value.

```js
const a = [1, 2];
a.fill(3);
// [3, 3]
```

```js
const a = ['a', 'b', 'c'];
a.fill('d');
// ['d'. 'd', 'd']
```

- What if we don't know how many "d"s we need in advance? First, we can create an array of a given size.

```js
const size = 1 + 2;
const arr = new Array(size);
arr.length;
// 3
```

- There's nothing in that array yet. If we ask for an element, we'll get `undefined`.

```js
const size = 1 + 2;
const arr = new Array(size);
arr[0];
// undefined
```

- Now that we created an array with a defined size, we can fill it with values. `new Array` and `.fill` are commonly used together for this exact reason.

```js
const size = 1 + 2;
new Array(size).fill('d');
// ['d', 'd', 'd']
```

- Now we can create a dynamically-sized filled array.

```js
function newAndFill(size, value) {
  return new Array(size).fill(value);
}
newAndFill(2, 'd');
// ['d', 'd']
```

Here's a concrete example of how we might use this technique. The progress bar at the top of this lesson is made of many `div`s. As the developers of Execute Program itself, we know how many paragraphs and code examples exist in the lesson. We create that many `div`s to form the progress bar. This is done using the `new Array(...).fill(...)` method shown here!

---

## review

```js
new Array(1000)[0];
// undefined
```

```js
0 in new Array(200).fill(true);
// true
```

```js
0 in new Array(5);
// false
```

```js
new Array(4).fill(100);
// [100, 100, 100, 100]
```

```js
new Array(0)[0];
// undefined
```

```js
0 in new Array(5).fill(null);
// true
```

```js
new Array(2).fill('Amir');
// ['Amir, 'Amir']
```

---

## quiz: "fill dynamically"

Write a function `fillDynamically(value, length)`. It should construct a new array of `length` filled with `value`. 

Given:

```js
function fillDynamically(value, length) {
  return ['d', 'd', 'd'];
}
```

My answer:

```js
return new Array(length).fill(value);
```

Author's answer:

```js
return new Array(length).fill(value);
```
