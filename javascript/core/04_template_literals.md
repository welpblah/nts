# core: template literals

JavaScript has always had two syntaxes for strings: we can use `'single quotes'` or `"double quotes"`. There's now a third type of string written with `` `backticks` ``. In simple cases, they're treated as ordinary strings: `` `hello` `` is the same as `'hello'`.

```js
`hello`;
// 'hello'
```

```js
`hello` === 'hello';
// true
```

These strings are called "template literals". They have several features that allow them to be used as "templates", with holes that are filled in later.

The most common use case is interpolation, which means "inserting something into something else". With template literals, we can insert the result of any JavaScript expression into the string by wrapping it in `${...}`.

```js
`1 + 1 = ${1 + 1}`;
// '1 + 1 = 2'
```

```js
`${'Shouting'.toUpperCase()} and ${'Whispering'.toLowerCase()}`;
// 'SHOUTING and whispering'
```

Interpolating with `${...}` converts the value to a string by calling its `.toString()` method. For numbers, that works great. But for arrays and objects, it probably won't do what we want.

```js
[1, 2].toString();
// '1,2'
```

```js
`two numbers: ${[1, 2]}`;
// 'two numbers: 1,2'
```

We can interpolate as many JavaScript expressions in one template literal as we like.

```js
const x = 4;
`1 + ${x} = ${x + 1}`;
'1 + 4 = 5'
```

Here's a code problem:

Write a function that shows us two numbers being added. Use template literals to build a string that includes the two numbers and the final sum. (Because you're building up strings, pay attention to the spacing!)

```js
function longSum(x, y) {
  return `${x} + ${y} = ${x + y}`;
}
[longSum(1, 2), longSum(10, 20)];
// ['1 + 2 = 3', '10 + 20 = 30']
```

All of the usual string syntax also works inside template literals. For example, inside of `'quotes'`, `\'` is an escaped single quote. We can also write `\'` inside template literals.

```js
'\'';
// "'"
```

```js
`\'`;
// "'"
```

Execute Program renders the string result identically no matter which syntax we use to define it. That's because template literals evaluate to regular strings when used in this way.

Normally, JavaScript strings can't have newlines in them:

```js
const x = 'oh
no'
// SyntaxError: on line 1: Unterminated string constant.
```

However, template literals don't have that limitation: they can contain newlines! This simplifies a lot of code. For example, here's an email template written using oldstyle JavaScript:

```js
onst name = 'Amir';
const email = [
  'Hi, ' + name,
  '',
  "We've updated our privacy policy!",
].join('\n');
email === "Hi, Amir\n\nWe've updated our privacy policy!";
// true
```

Here's a version with template literals. It's much cleaner: we don't have to constantly open and close quotes, and we don't have to merge the lines with `join`.

```js
const name = 'Amir';
const email = `
  Hi, ${name},
  
  We've updated our privacy policy!
`;
email === "\n  Hi, Amir,\n  \n  We've updated our privacy policy!\n";
// true
```

There is one important difference between the two examples above. In the template literal version, the string includes all of the whitespace between the opening and closing backtick. That includes the newlines, which we wanted. But it also includes the indentation at the beginning of the individual lines, which we may not want.

There are ways to remove that leading whitespace, like the [dedent](https://www.npmjs.com/package/dedent) NPM module. But sometimes the whitespace doesn't matter. For example: in most cases, whitespace between HTML tags won't cause any problems, so we can leave it in.

---

## review

```js
`2 + 2 = ${2 + 2}`;
// '2 + 2 = 4'
```

Write a function that shows us two numbers being added. Use template literals to build a string that includes the two numbers and the final sum. (Because you're building up strings, pay attention to the spacing!)

```js
function longSum(x, y) {
  return `${x} + ${y} = ${x + y}`;
}
[longSum(1, 2), longSum(10, 20)];
// ['1 + 2 = 3', '10 + 20 = 30']
```

```js
const name = 'Betty';
const email = `
  Hi, ${name},
  
  We've been acquired and are shutting down!
`;
email === "\n  Hi, Betty,\n  \n  We've been acquired and are shutting down!\n";
// true
```

```js
`some + thing = ${'some' + 'thing'}`;
// 'some + thing = something'
```

Write a function that shows us two numbers being added. Use template literals to build a string that includes the two numbers and the final sum. (Because you're building up strings, pay attention to the spacing!)

```js
function longSum(x, y) {
  return `${x} + ${y} = ${x + y}`;
}
[longSum(1, 2), longSum(10, 20)];
//  ['1 + 2 = 3', '10 + 20 = 30']
```