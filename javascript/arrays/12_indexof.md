## index of

- We can ask an array for the index of a particular value.

```js
onst abc = ['a', 'b', 'c'];
abc.indexOf('a');
// 0
```

- If the value occurs multiple times, we'll only get the index of the first occurrence.

```js
const abc = ['a', 'b', 'c', 'c'];
abc.indexOf('c');
// 2
```

- If we ask for an element that isn't in the array, we get `-1`.

```js
const abc = ['a', 'b', 'c'];
abc.indexOf('d');
// -1
```

 It's important to check your `.indexOf` calls for that `-1` return value! Otherwise you can introduce subtle bugs. Here's an example.

 Let's try to slice an array from a certain element forward. To do that, we'll first grab the element's index with `.indexOf`. 

 ```js
const abc = ['a', 'b', 'c'];
abc.slice(abc.indexOf('b'));
// ['b', 'c']
```

This seems to work. But, what if the element we ask for isn't in the array? Then, we'll get back a `-1` from `.indexOf`, and then pass it as an index to `.slice`.

(A hint in case you get stuck: `[1, 2, 3].slice(-1)` returns `[3]`.)

```js
const abc = ['a', 'b', 'c'];
abc.slice(abc.indexOf('c'));
// ['c']
```

```js
const abc = ['a', 'b', 'c'];
abc.slice(abc.indexOf('d'));
// ['c']
```

That isn't what we intended: there's no 'd' in the array, so we should return some value indicating that fact. For example, we might want to return [], or undefined, or null. But we definitely don't want to return ['c']!

We can fix that bug by checking for `-1`.

```js
const abc = ['a', 'b', 'c'];
function sliceFromElement(array, element) {
  const index = array.indexOf(element);
  if (index === -1) {
    return null;
  } else {
    return array.slice(index);
  }
}
sliceFromElement(abc, 'd');
// null
```

### review

```js
const abc = [100, 200, 300];
abc.indexOf(300);
// 2
```

```js
const abc = [true, true, true];
abc.indexOf(false);
// -1
```

```js
const abc = [true, false, true];
abc.indexOf(false);
// 1
```

```js
const abc = ['Amir', 'Cindy'];
abc.indexOf('Betty');
// -1
```
