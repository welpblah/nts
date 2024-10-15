# concurrency: setTimeout and Functions

`setTimeout` works in the same way when we wrap our code in functions, classes, or any other language construct.

```js
function doAsyncWork() {
  const array = [];
  setTimeout(() => array.push('it worked'), 1000);
  return array;
}
const array = doAsyncWork();
array;
// ['it worked']
```

As our examples get more complex, we'll want to extract our asynchronous callbacks into their own functions. Here's an example of that: we asynchronously collect users' names in an array.

```js
const users = [
  {name: 'Amir'},
  {name: 'Betty'},
];

function addUserName(names, user) {
  names.push(user.name);
}

function doAsyncWork() {
  const names = [];
  for (const user of users) {
    setTimeout(() => addUserName(names, user), 1000);
  }
  return names;
}

const array = doAsyncWork();
array;
// ['Amir', 'Betty']
```

Extracting callbacks like this makes changing code easier. If we're tracking down a bug where asynchronous code seems to execute in the wrong order, we'll start by looking at `doAsyncWork`. But if we're fixing a bug in the work itself, we'll start by looking at `addUserName`.

(Of course, real-world equivalents of `addUserName` will be more than one line long. Imagine that function being 10 lines long instead of 1!)

Here's a code problem:

Set a timer to call the `addMessage` function after 100 ms.

(This example uses `.slice()` to duplicate the `messages` array's value before any timers run. That lets us check the value of the array before and after the timer fires.)

```js
const messages = [];
function addMessage() {
  messages.push('It worked!');
}

// code here
setTimeout(addMessage, 100);
// end here

const initialMessages = messages.slice();
[initialMessages, messages];
// [[], ['It worked!']]
```

---

## review

```js
function loadAsyncData() {
  const array = [];
  setTimeout(() => array.push('loaded'), 1000);
  return array;
}
const array = loadAsyncData();
array;
// ['loaded']
```

```js
function writeToDatabase() {
  const array = [];
  setTimeout(() => array.push('data was written'), 1000);
  return array;
}
const array = writeToDatabase();
// ['data was written']
```

Set a timer to call the `addMessage` function after 100 ms.

(This example uses `.slice()` to duplicate the `messages`array's value before any timers run. That lets us check the value of the array before and after the timer fires.)

```js
const messages = [];
function addMessage() {
  messages.push('It worked!');
}

// code here
setTimeout(addMessage, 100);
// end

const initialMessages = messages.slice();
[initialMessages, messages];

// [[], ['It worked!']]
```

