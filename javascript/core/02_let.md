# core: let

Before 2015, local variables in JavaScript were declared with the `var` keyword. When we define a `var` inside a function, it's only visible inside the function. We're allowed to use `var` in strict mode.

```js
function defineX() {
  var x = 1;
  return x + 1;
}
defineX();
// 2
```

However, trying to reference the variable outside of the function is an error.

Here's a code problem:

Reference `x` outside of the function, which will cause an error. For example, you can use a `console.log` or assign `x` a new value.

```js
function defineX() {
  var x = 1;
}
defineX();

// My answer:
x;

// Author's answer:
console.log(x);

// Goal: ReferenceError: x is not defined
```

Functions create a variable scope. Variables defined inside the function are visible within the function, and invisible outside the function. This is good!

We expect an `if` block to behave the same way. If we put a `var` inside an `if (...) { ... }`, we expect the variable to be visible inside the `if` block, which it is:

```js
function f() {
  if (true) {
    var x = 1;
    return x + 1;
  }
}
f();
// 2
```

However, the variable is also visible to the rest of the function body, even outside of the `if` block!

Here's a code problem:

Modify the function to return `x + 1`.

```js
function f() {
  if (true) {
    var x = 1;
  }
  // My answer:
  return x + 1;
}
f();
// 2
```

All `var`s are "function-scoped", which means that they're visible to the entire function body, no matter how they're defined within the function. This was a mistake in JavaScript's design: `var x = 1` inside an `if` shouldn't be visible outside the `if`. It's too easy to declare a variable, thinking that it will be local to the `if`, then accidentally use it later in the function.

Fortunately, this problem was fixed in 2015, when `let` was introduced. With `let`, a variable defined inside the `if` isn't visible outside the `if`. Trying to access it will cause an error. (You can type `error` when a code example will throw an error.)

```js
function f() {
  if (true) {
    let x = 1;
  }
  return x;
}
f();
// ReferenceError: x is not defined
```

`let` handles nested scopes properly. For example, we can define an `x` in the function body, then define another `x` inside an `if`. Changing the "inner" `x` won't change the "outer" `x`.

```js
function f() {
  let x = 'outer';
  if (true) {
    let x = 'inner';
  }
  return x;
}
f();
// 'outer'
```

Those variables hold different values even though they have the same name. That's called "shadowing": the inner `let x` shadows the outer `let x`. Opinions vary on whether shadowing should be used sparingly, or avoided altogether. Our opinion is: use it sparingly, and only when all of the alternatives feel awkward.

We've been using `if` for our examples, but `let` scoping rules apply to any block of code in curly braces, like `{ ... }`. For example, an outer scope can't access a variable defined inside a `while`; that causes an error.

```js
function f() {
  let iterating = true;
  while (iterating) {
    let x = 1;
    iterating = false;
  }
  return x;
}
f();
// ReferenceError: x is not defined
```

We can also introduce new scopes without using a construct like `if` or `while`. Any code contained in `{ ... }` will have its own scope.

```js
function f() {
  let x = 'outer';
  {
    let x = 'inner';
  }
  return x;
}
f();
// 'outer'
```

Given that `let` has clearer and less error-prone behavior than `var`, we recommend that you always use `let` instead of `var` when declaring variables.

---

## review

```js
function getUser() {
  if (true) {
    let user = {name: 'Amir', age: 36};
  }
  return user;
}
getUser();
// ReferenceError: user is not defined
```

```js
function getUserName() {
  let user = {name: 'Amir'};
  if (true) {
    let user = {name: 'Betty'};
  }
  return user.name;
}
getUserName();
// 'Amir'
```

```js
function double(x) {
  if (true) {
    let doubled = 2 * x;
  }
  return doubled;
}
double(3);
// ReferenceError: doubled is not defined
```

```js
function f() {
  let x = 'outer';
  if (true) {
    let x = 'inner';
  }
  return x;
}
f();
// 'outer'
```