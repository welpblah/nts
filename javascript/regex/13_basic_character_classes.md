# regex: basic character classes

- Some sets of characters occur together frequently. For example, we often need to recognize numbers (the digits 0-9). We can use `\d` for that. It recognizes 0, 1, ..., 8, 9.

- We use `\s` to recognize whitespace (spaces, newlines, tabs, etc.) There are more of these, but we'll start with `\s` and `\d`. These are called _character classes_.

```js
/\s/.test(' ');
// true
/\s/.test('\n');
// true
/\s/.test('');
// false
/\s/.test('x\ty'); /* \t is a tab character */
// true
/\s/.test('cat');
// false
/\d/.test('0');
// true
/\d/.test('9');
// true
/\d/.test('a');
// false
```

- Making these character classes upper-case negates them. For example, `\d` is all digits. But `\D` is any character that isn't a digit.

```js
/\D/.test('0');
// false
/\S/.test(' ');
// false
/\S/.test('0');
// true
```

- A character class matches only one character in the string. If we want to match multiple characters, we can use `+` or `*`.

```js
/^\d$/.test('1000');
// false
/^\d*$/.test('3000');
// true
/^\d*$/.test('1000 cats');
// false
/^\d*$/.test('cats');
// false
/^\d*$/.test('');
// true
```

---

## review #1

```js
/\s/.test(' ');
// true
```

## review #2

```js
/\s/.test('\n');
// true
```

## review #3

```js
/\s/.test('');
// false
```

## review #4

```js
/\d/.test('9');
// true
```

## review #5

```js
/\S/.test(' ');
// false
```

## review #6

```js
/\D/.test('0');
// false
```

## review #7

```js
/^\d$/.test('1000');
// false
```

## review #8

```js
/^\d*$/.test('3000');
// true
```

## review #9

```js
/\s/.test(' ');
// true
```

## review #10

```js
/\s/.test('\n');
// true
```

## review #11

```js
/\s/.test('');
// false
```

## review #12

```js
/\D/.test('0');
// false
```

## review #13

```js
/\S/.test(' ');
// true
```

## review #14

```js
/^\d*$/.test('3000');
// true
```

## review #15

```js
/\s/.test('\n');
// true
```

## review #16

```js
/^\d$/.test('1000');
// false
```

## review #17

```js
/[^ab]/.test('c');
// true
```

## review #18

```js
/[^a-z]/.test('5');
// true
```

## review #19

```js
/\s/.test('\n');
// true
```
