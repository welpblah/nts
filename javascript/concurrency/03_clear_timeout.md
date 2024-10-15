# concurrency: clearTimeout

We've seen that `setTimeout` schedules a function to run in the future. But what if we want to cancel a timer that we've already created? Fortunately, we can do that with the `clearTimeout` function.

In browsers, `setTimeout` always returns a number, which is the ID of the timer. NodeJS behaves a bit differently: there, `setTimeout` returns an object. In both cases, we can pass `setTimeout`'s return value to `clearTimeout`, which will cancel the timer. We just have to remember to store `setTimeout`'s return value in a variable.

```js
const array = [];
const timer = setTimeout(() => { array.push('timer'); }, 1000);
clearTimeout(timer);
array;
// []
```

```js
const array = [];

const timer1 = setTimeout(() => { array.push('timer 1'); }, 1000);
const timer2 = setTimeout(() => { array.push('timer 2'); }, 1000);

clearTimeout(timer2);

array;
// ['timer 1']
```

What if we call `clearTimeout` on a timer that has already fired? There's no longer a timer to clear, so it silently does nothing.

```js
const array = [];

const timer1 = setTimeout(() => { array.push('timer 1'); }, 1000);
const timer2 = setTimeout(() => { array.push('timer 2'); }, 1000);

clearTimeout(timer2);
clearTimeout(timer2);

array;
// ['timer 1']
```

`clearTimeout` doesn't mind if we pass in something that's not a timer. For example, calling it with `undefined` won't throw an exception.

```js
clearTimeout(['not', 'a', 'timer']);
clearTimeout(undefined);
// undefined
```

Here's a code problem:

Clear the timer to prevent it from running.

```js
const array = [];
const timer = setTimeout(() => { array.push('it ran'); }, 100);

// code here
clearTimeout(timer);
// end here

array;
// []
```

Canceling timers is relatively uncommon, but it eventually comes up in most production applications. Here's an example.

Imagine that we're building a client-side web application in a security-sensitive industry like banking. We need to log the user out automatically after five minutes of inactivity. Every time the user takes an action, like clicking a button or typing into a text box, we want to restart the five-minute inactivity countdown. (Being occasionally logged out is annoying to users, but it also reduces the chance of someone leaving their account logged in on a public computer!)

Here's some code expressing that idea. We won't actually run this code; it's just a sketch of a solution.

```js
const inactivity = {
  timer: undefined,
  reset() {
    clearTimeout(this.timer);
    const fiveMinutes = 5 * 60 * 1000;
    this.timer = setTimeout(logOut, fiveMinutes);
  }
};

inactivity.reset();
```

`clearTimeout` makes that code relatively simple. We just need to provide a `logOut` function, and we need to modify our application to call `inactivity.reset()` whenever the user clicks a button, etc.

Now our users are slightly annoyed, but some are safer!

---

## review

```js
const array = [];
const timer = setTimeout(() => { array.push('it ran'); }, 1000);
clearTimeout(timer);
array;
// []
```

```js
const array = [];
const t = setTimeout(() => { array.push('timer ran'); }, 1000);
clearTimeout(t);
array;
// []
```

---
Clear the timer to prevent it from running.

```js
const array = [];
const timer = setTimeout(() => { array.push('it ran'); }, 100);

// code here
clearTimeout(timer);
// end

array;
// []
```
