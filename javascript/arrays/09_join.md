# join

- Sometimes we want to turn an array of strings into a single string. We can `.join` them together.

```js
['a', 'b', 'c'].join('');
// 'abc'
```

```js
['Amir', 'Betty'].join('');
// 'AmirBetty'
```

- `.join` takes one argument: the string to put between each array element, called the "separator". If we call it without an argument, the strings are joined with `','` by default.

```js
['Amir', 'Betty'].join();
// 'Amir,Betty'
```

- The separator can be any string.

```js
['a', 'b', 'c'].join('x');
// 'axbxc'
```

```js
['a', 'b', 'c'].join('AND');
// 'aANDbANDc'
```

- If we join some empty strings together, we get a string of only commas. You can think about it as: the string has all of the commas in the original array.

```js
['', ''].join();
// ','
```

```js
['', '', ''].join();
// ',,'
```

```js
['a', '', 'b'].join();
// 'a,,b'
```

- If we try to join an array that includes number or boolean elements, JavaScript converts them to strings.

```js
[1, 2, 3].join('-');
// '1-2-3'
```

```js
[true, 3, false].join('.');
// 'true.3.false'
```

```js
['w', 'd', 40].join('-');
// 'w-d-40'
```

- The same thing happens if we use a number or boolean as the separator. (You'll probably never do this intentionally, but you may do it accidentally!)

```js
['a', 'b', 'c'].join('1');
// 'a1b1c'
```

```js
[4, 5].join(false);
// '4false5'
```

```js
[1,2,3].join(4);
// '14243'
```

- `null` and `undefined` become empty strings when `.join`ed.

```js
[null, undefined].join();
// ','
```

```js
['a', null, undefined, 'b'].join();
// 'a,,,b'
```

- `.join` comes up in many different situations. For example, we can use it to build a large string using short lines of code.

```js
function userTag(name) {
  return [
    '<User name="',
    name,
    '">'
  ].join('');
}
userTag('Amir');
// '<User name="Amir">'
```

- We can also `.join` to turn an array of words into a readable sentence.

```js
const foods = ['peas', 'carrots', 'potatoes'];
foods.slice(0, -1);
// ['peas', 'carrots']
```

```js
const foods = ['peas', 'carrots', 'potatoes'];
const joined = [
  foods.slice(0, -1).join(', '),
  ', and ',
  foods[foods.length - 1],
].join('');
joined;
// 'peas, carrots, and potatoes'
```

## review #1

```js
['Cindy', 'Dalili'].join();
// 'Cindy,Dalili'
```

## review #2

```js
['d', 'e', 'f'].join('_');
// 'd_e_f'
```

## review #3

```js
['first', undefined, null, 'last'].join();
// 'first,,,last'
```

## review #4

```js
['Ebony', 'Fang'].join();
'Ebony,Fang'
```

## review #5

```js
['1', '2', '3'].join('-');
// '1-2-3'
```

## review #6

```js
['x', null, 'y', undefined].join();
// 'x,,y,'
```

## review #7

```js
['a', 'b', 'c'].join('x');
// 'axbxc'
```
