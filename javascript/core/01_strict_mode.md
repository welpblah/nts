# core: strict mode

Variable scoping has often been a sore point in JavaScript. The simplest assignment syntax, `x = 1`, defines a global variable. That's rarely what we want!

Fortunately, modern versions of JavaScript have "strict mode". It prevents many kinds of mistakes, including global variable definitions like `x = 1`. Strict mode is enabled by putting the string `'use strict'` at the top of a module or function.

Babel, TypeScript, and most other JavaScript compilers automatically insert `'use strict'` for us. This course does that as well: all code here runs in strict mode. If we try to define a variable with `x = 1`, we'll get an error. (You can type `error` when a code example will throw an error.)

```js
function defineX() {
  x = 1;
  return 'ok';
}
defineX();
// ReferenceError: x is not defined
```

Note that we didn't reference `x` after trying to define it. The `ReferenceError` is happening simply because we said `x = 1`.

Scoping rules aren't a flashy part of JavaScript, so why start a "Modern JavaScript" course here? Because strict mode marks the beginning of a decade-long (and continuing) push to modernize the JavaScript language. The first step in modernization was to disallow some of the old, confusing behavior, like `x = 1` creating a global variable.

Let's take a look at two more strict mode examples before moving on to other new features in JavaScript.

JavaScript supports numbers in octal (base 8). For example, `0100` in octal is 64 in base 10. However, octal is rarely used; if you type `0100`, it's more likely that you meant `100`! To prevent that mistake, all uses of octal cause errors in strict mode.

```js
64 === 0100
// SyntaxError: on line 1: Legacy octal literals are not allowed in strict mode.
```

One final strict mode example. The `delete` keyword can be used to completely remove a key from an object.

```js
const user = {name: 'Amir', age: 36};
delete user.age;
user;
// {name: 'Amir'}
```

In the past, the `delete` keyword could also delete entire variables: `delete user`. However, in strict mode, trying to do that causes an error.

```js
const user = {name: 'Amir', age: 36}
delete user
// SyntaxError: on line 2: Deleting local variable in strict mode.
```

Strict mode disallows `delete` on variables because deleting variables is confusing and unnecessary. If we create a variable inside a function, we expect to be able to reference it at the end of that function. If we want the variable to exist for some code, but not other code, then it's better to break those sections of code into two functions.

Certain object properties are also undeletable. For example, trying to delete the `Object.prototype` property will cause an error. (Deleting that property would wreak havoc on JavaScript's prototype-based object system.)

```js
delete Object.prototype;
// TypeError: Cannot delete property 'prototype' of function Object() { [native code] }
```

Now that we know how to avoid some of JavaScript's most confusing features, we're ready to explore the new features that are helpful!

---

## review

```js
function defineSomething() {
  aVariable = 1;
}
defineSomething();
// ReferenceError: aVariable is not defined
```

```js
const aNumber = 55
delete aNumber;
// SyntaxError: on line 2: Deleting local variable in strict mode.
```

```js
function defineAGlobal() {
  someGlobalVariable = 1;
  return 'ok';
}
defineAGlobal();
// ReferenceError: someGlobalVariable is not defined
```

```js
const user = {name: 'Amir', age: 36}
delete user
// SyntaxError: on line 2: Deleting local variable in strict mode.
```