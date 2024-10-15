# core: const

Sometimes we want to ensure that a variable is never reassigned. We do that by declaring it with `const` rather than `let`. With `const`, trying to give the variable a new value causes an error. (You can type `error` when a code example will throw an error.)

```js
function f() {
  const x = 1;
  return x + 1;
}
f();
// 2
```

```js
function f() {
  const x = 1;
  x = 2;
  return x + 1;
}
f();
// TypeError: Assignment to constant variable.
```

Here's a code problem:

Change the `let` here to a `const`. That will cause an error because the variable is reassigned.

```js
function f() {
  const x = 1;
  x = 2;
  return x;
}
// TypeError: invalid assignment to const 'x'
```

`const` doesn't stop us from mutating (changing) the value held by the variable. For example, we can mutate a `const` array by calling its `push` method. However, reassigning the array variable to hold a new array isn't allowed.

Here's a code problem:

Use numbers.push(...) to push a 3 onto the array. That's allowed even though we declared numbers with const.

```js
function f() {
  const numbers = [1, 2];

  // code here
  numbers.push(3);

  return numbers;
}
f();
// numbers.push(3);
```

Here's a code problem:

Assign a new value to `numbers` by doing `numbers = /* some value here */`. That will cause an error because `numbers` is `const`.

```js
function f() {
  const numbers = [1, 2];

  // code
  numbers = [3];
  // numbers = [1, 2, 3];

  return numbers;
}
f();
// TypeError: invalid assignment to const 'numbers'
```

The short version is: `const` applies to the variable, not the value held by the variable.

We also can't define the same variable twice; that will cause an error.

```js
const x = 'first'
const x = 'second'
// SyntaxError: on line 2: Identifier 'x' has already been declared.
```

However, we can shadow `const` variables, like we can with `let`.

```js
function f() {
  const x = 'outer';
  {
    const x = 'inner';
  }
  return x;
}
f();
// 'outer'
```

Variable scoping isn't the flashiest topic. But we declare, use, and think about variables more than almost anything else! It's important for us to get it right.

Prior to 2015, JavaScript's variable scopes were a minefield that made errors easy. Today, the minefield is still there: `var` is still part of the language and it won't be going away. But we can avoid most errors by fully switching to `let` and `const`.

(Tools like [ESLint](https://eslint.org/) can help you remember to use `let`. You can configure your linter to [disallow `var` completely](https://eslint.org/docs/rules/no-var). If you like, it can also [force you](https://eslint.org/docs/rules/prefer-const) to use `const` for variables that are never reassigned. We recommend both.)

`let` and `const` nicely encapsulate the goals of ECMAScript 5, ECMAScript 2015, and newer versions: to modernize the language and fix past mistakes. (ECMAScript is another name for JavaScript. It's primarily used when talking about the official language specification documents.)

`let` and `const` might not seem like revolutionary new features. But they fixed a twenty-year-old design mistake in JavaScript, and have made the language more suitable for large-scale application development.

---

## review

```js
function getUser() {
  const user = {name: 'Amir'};
  user = {name: 'Betty'};
  return user;
}
getUser();
// TypeError: Assignment to constant variable.
```

```js
function getUserName() {
  const user = {name: 'Amir'};
  {
    const user = {name: 'Betty'};
  }
  return user.name;
}
getUserName();
// 'Amir'
```

```js
function quadruple(x) {
  const result = 2 * x;
  result = 2 * result;
  return result;
}
quadruple(3);
// TypeError: Assignment to constant variable.
```

```js
function f() {
  const x = 'outer';
  {
    const x = 'inner';
  }
  return x;
}
f();
// 'outer'
```
