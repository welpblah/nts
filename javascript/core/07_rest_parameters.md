# core: rest parameters

JavaScript allows functions to take a variable number of parameters. In recent versions of JavaScript, we can do that by adding `...` to the parameter list. All the arguments we pass in show up in a single array.

```js
function f(...args) {
  return args;
}
f('a', 'b');
// ['a', 'b']
```

```js
function f(...args) {
  return args;
}
f(1, 2, 3);
// [1, 2, 3]
```

In JavaScript, this feature is called "rest parameters" because it collects "the rest" of the function's parameters into an array. In other languages, you may see it called "varargs", which stands for "VARiable number of ARGuments".

```js
function max(...numbers) {
  let max = numbers[0];
  for (const n of numbers) {
     if (n > max) {
       max = n;
     }
  }
  return max;
}
max(1, 2, 3);
// 3
```

Here's a code problem:

Write a function `sum` that sums numbers. It should take the numbers as rest parameters. If no arguments are given, it should return 0.

My answer:

```js
function sum(...nums) {
  let sum = 0;
  nums.forEach(n => { sum += n; });
  return sum;
}

const sums = [sum(), sum(100), sum(2000, 1), sum(-500, -300)];
sums;

// [0, 100, 2001, -800]
```

Author's answer:

```js
function sum(...numbers) {
  let total = 0;
  for (const n of numbers) {
    total += n;
  }
  return total;
}

const sums = [sum(), sum(100), sum(2000, 1), sum(-500, -300)];
sums;
// [0, 100, 2001, -800]
```

Rest parameters can be used after regular positional parameters.

```js
function addMany(toAdd, ...numbers) {
  const result = [];
  for (const n of numbers) {
    result.push(n + toAdd);
  }
  return result;
}
addMany(2, 1, 7.7, 1000);
// [3, 9.7, 1002]
```

However, positional parameters can't be used after rest parameters. Trying to do that is an error! (You can type `error` when a code example will throw an error.)

```js
function addMany(...numbers, toAdd) {
  const result = [];
  for (const n of numbers) {
    result.push(n + toAdd);
  }
  return result;
}
addMany(1, 7.7, 1000, 2);
// SyntaxError: on line 1: Rest element must be last element.
```

We also can't have multiple rest parameters. The JavaScript virtual machine would have no way to know which argument values go in which rest parameter. Trying to do that also causes an error.

```js
function multiplyArrays(...numbers1, ...numbers2) {
  const result = []
  for (let i = 0; i < numbers1.length; i++) {
    result.push(numbers1[i] * numbers2[i])
  }
  return result
}
multiplyArrays(1, 2, 3, 4)
// SyntaxError: on line 1: Rest element must be last element.
```

So far we've seen rest parameters in function definitions. They also work when calling a function. When we call `f(...args)`, it means "pass the elements of the `args` array as separate parameters."

```js
function add(x, y) {
  return x + y;
}
const numbers = [1, 2];
add(...numbers);
// 3
```

---

## review

Write a function `sum` that sums numbers. It should take the numbers as rest parameters. If no arguments are given, it should return 0.

```js
function sum(...numbers) {
  let sum = 0;
  numbers.forEach(n => { sum += n });
  return sum;
}

// function sum(...numbers) {
//   let total = 0;
//   for (const n of numbers) {
//     total += n;
//   }
//   return total;
// } 

const sums = [sum(), sum(100), sum(2000, 1), sum(-500, -300)];
sums;
// [0, 100, 2001, -800]
```

