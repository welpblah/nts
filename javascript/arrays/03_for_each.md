# arrays: for each

- A regular `for ... of` loop gives us each element of the array, one by one.

```js
const numbers = [1, 2, 3];
let sum = 0;
for (const n of numbers) {
  sum = sum + n;
}
sum;
// 6
```

- The `.forEach` array method is similar. We pass a callback function to `forEach`, and it's called once for each array element. In the example below, the callback function is `n => { ... }`.

```js
const numbers = [1, 2, 3];
let sum = 0;
numbers.forEach(n => {
  sum = sum + n;
});
sum;
// 6
```

```js
const people = [
  {name: 'Cindy'},
  {name: 'Dalili'},
];

const names = [];
people.forEach(person => {
	names.push(person.name);
});
// ['Cindy'], ['Dalili']
```

- Note that in the last two examples, the callback modified a variable defined outside the `.forEach`. JavaScript functions can always modify variables defined in outer scopes, so this is nothing special. But it's a common pattern with `.forEach` loops.

- We can also modify the array's elements during the `.forEach`.

```js
const people = [
  {name: 'Ebony'},
  {name: 'Fang'},
];
people.forEach(person => {
  person.name = person.name.toUpperCase();
});
people[0].name;
// 'EBONY'
```

- The callback function passed to `.forEach` takes a second, optional argument: the element's index.

```js
const names = ['Amir', 'Betty'];
const userIDs = [10, 11];
let result = '';
names.forEach((name, index) => {
  result += name + userIDs[index];
});
result;
// 'Amir10Betty11'
```

- This is one reason that `.forEach` is useful. It can give us the array indexes, like a normal `for` loop does. And it can give us the array values, like a `for ... of` loop does. But unlike ether of those loops, `.forEach` can give us the index and value at the same time.

- So far, we defined the callback function inline, right at the point where we called `.forEach`. Because functions are values in JavaScript, we can also define the callback outside the `.forEach` loop, then pass it in.

```js
let sum = 0;
function addToSum(n) {
  sum += n;
}
[1, 2, 3, 4].forEach(addToSum);
sum;
// 10
```

- The same thing works with an arrow function.

```js
let sum = 0;
const addToSum = n => sum += n;
[1, 2, 3, 4].forEach(addToSum);
sum;
// 10
```

- It's tempting to think of `.forEach` like a regular `for` or `for ... of` loop, but that will lead to mistakes. When we write a `for` or `for ... of` loop, there's no callback function. If we `return` in the loop, that returns from the outer function that contains the loop.

```js
function hasTheLetterB(letters) {
  for (const letter of letters) {
    if (letter === 'b') {
      return true;
    }
  }
  
  return false;
}

hasTheLetterB(['o', 'r', 'b']);
// true
```

- We can convert `hasTheLetterB` to use `.forEach`, which requires us to add a callback function. Now the `return true` is inside of the callback function. But that return value is thrown away on every iteration of the loop, so it has no effect. That's a problem! Our function always returns `false`, even when the array contains a 'b'.

```js
function hasTheLetterB(letters) {
  letters.forEach(letter => {
    if (letter === 'b') {
      return true;
    }
  });
  
  return false;
}

hasTheLetterB(['o', 'r', 'b']);
// false
```

- We can fix this by declaring a variable outside the loop to keep track of whether we encountered the letter 'b'. Then we return this variable at the end of the function, like we did with `sum` earlier.

```js
function hasTheLetterB(letters) {
  let sawLetterB = false;
  
  letters.forEach(letter => {
    if (letter === 'b') {
      sawLetterB = true;
    }
  });
  
  return sawLetterB;
}

hasTheLetterB(['o', 'r', 'b']);
// true
```

---

## code problem 1/2

The function below decides whether any number in the array is positive. Currently it's broken: it always returns `false`. That's because we tried to `return true` inside the `.forEach` callback function. But the callback's return value is ignored, so that doesn't work.

Modify the function so it works as intended. We declared a `sawPositiveNumber` variable for you outside the loop, which you can use to track whether the loop has seen a positive number or not. Remember to return it at the end!

Given:

```js
function hasPositiveNumbers(numbers) {
  let sawPositiveNumber = false;
  numbers.forEach(n => {
    if (n > 0) {
      return true;
    }
  });
  return false;
}
```

My answer:

```js
function hasPositiveNumbers(numbers) {
  let sawPositiveNumber = false;
  numbers.forEach(n => {
    if (n > 0) {
      sawPositiveNumber = true;
    }
  });
  return sawPositiveNumber;
}
```

Author's answer:

```js
function hasPositiveNumbers(numbers) {
  let sawPositiveNumber = false;
  numbers.forEach(n => {
    if (n > 0) {
      sawPositiveNumber = true;
    }
  });
  return sawPositiveNumber;
}
```

## code problem 2/2

Write a function `count` that takes an array and a callback function, then returns the number of elements where `callback` returns `true`. Use `.forEach` to iterate through the array.

A hint: you'll need a variable outside the `.forEach` loop to keep track of the element count. Remember to return it at the end!

Given: 

```js
function count(arr, callback) {
  arr.forEach(element => {
}
```

My answer: 

```js
function count(arr, callback) {
  let count = 0;
  arr.forEach(element => {
    if(callback(element)) {
      count++;
    }
  });
  return count;
}
```

Author's answer:

```js
function count(arr, callback) {
  let elementCount = 0;
  arr.forEach(e => {
    if (callback(e)) {
      elementCount += 1;
    }
  });
  return elementCount;
}
```

---

## review #1

```js
const numbers = [10, 20, 30];
let sum = 0;
numbers.forEach(n => {
  sum = sum + n;
});
sum;
// 60
```

## review #2

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

## review #3

```js
const numbers = [4, 5, 6];
let sum = 0;
numbers.forEach(n => {
  sum = sum + n;
});
sum;
// 15
```

## review #4

Write a function `count` that takes an array and a callback function, then returns the number of elements where `callback` returns `true`. Use `.forEach` to iterate through the array.

A hint: you'll need a variable outside the `.forEach` loop to keep track of the element count. Remember to return it at the end!

Given:

```js
function count(arr, callback) {
  arr.forEach(element => {
}
```

My answer:

```js
function count(arr, callback) {
  let count = 0;
  arr.forEach(element => {
    if(callback(element)) {
      count++;
    }
  });
  return count;
}
```

Author's answer:

```js
function count(arr, callback) {
  let elementCount = 0;
  arr.forEach(e => {
    if (callback(e)) {
      elementCount += 1;
    }
  });
  return elementCount;
}
```

## review #5

```js
const people = [
  {name: 'Cindy'},
  {name: 'Dalili'},
];

const names = [];
people.forEach(person => {
  names.push(person.name);
});
names;
// ['Cindy', 'Dalili']
```

## review #6
 
Write a function `count` that takes an array and a callback function, then returns the number of elements where `callback` returns `true`. Use `.forEach` to iterate through the array.

A hint: you'll need a variable outside the `.forEach` loop to keep track of the element count. Remember to return it at the end!

```js
function count(arr, callback) {
  let count = 0;
  arr.forEach(element => {
    if (callback(element)) {
      count += 1;
    }
  });
  return count;
}
```

## review #7

```js
const people = [
  {name: 'Cindy'},
  {name: 'Dalili'},
];

const names = [];
people.forEach(person => {
  names.push(person.name);
});
names;
// ['Cindy', 'Dalili']
```
