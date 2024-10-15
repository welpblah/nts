## concurrency: concurrent setTimeoutes

`setTimeout` can have a delay of 0, which means "do this immediately". However, it's still asynchronous! Whenever we use a `setTimeout` with any delay, even 0, the JavaScript engine will finish running all the synchronous code before calling the `setTimeout` callback.

```js
console.log('first');
setTimeout(() => console.log('second'), 0);
console.log('third');
// 53ms first
// 54ms third
// 62ms second
```

Timers are asynchronous regardless of how many we create. When we create 3 timers, each with a delay of 0, they will only run once the current code finishes. Timers with a delay of 0 will always execute in the order that they were created.

(In this example, remember that `'first'` and `'third'` will be `push`ed onto the array before the timers run.)

```js
const array = [];
array.push('first');
for (const i of [1, 2]) {
  setTimeout(() => array.push('second ' + i), 0);
}
array.push('third');
array;
// ['first', 'third', 'second 1', 'second 2']
```

Since the timeout is 0, `'second 1'` was `push`ed to the array immediately after the other code finished running. What happens when we create timers with timeouts greater than 0? Each will fire when its timeout is reached. In the example below, the `'cat'` timer will fire before the `'dog'` timer.