# regex: literals

- Regular expressions (regexes) are patterns that describe strings. We might write a regex for filenames ending in ".jpg". Or we might write one that recognizes phone numbers. We'll use JavaScript's regexes, but it's OK if you don't know JavaScript.

- We can ask whether a regex matches a given string: `/a/.test('cat')`. The regex `/a/` means "any string containing an a". We asked about `'cat'`, which contains an "a", so we'll get `true` back.

```js
/a/.test('a');
// true
```

```js
/a/.test('b');
// false
```

```js
/a/.test('xax');
// true
```

- Regexes are case-sensitive, so "a" and "A" are different characters.

```js
/A/.test('a');
// false
```

```js
/A/.test('A');
// true
```

- Multi-character regexes test that the characters appear together. They have to be adjacent, with no other characters between them.

```js
/cat/.test('cart');
// false
```

```js
/cat/.test('cat');
// true
```

```js
/cat/.test('the cat there');
// true
```

- Spaces are treated as normal characters. If they're in the regex, they'll have to be in the matched string too. Likewise for `_`, `-`, and some other punctuation. (But some punctuation has special meaning, which we'll see soon.)

```js
/a cat/.test('that is a cat');
// true
```

```js
/_/.test('happy-cat');
// false
```

```js
/_/.test('happy_cat');
// true
```

- Write a regex that matches any string containing "cat".

```js
var re = /cat/;
```

---

## review #1

```js
/c/.test('d');
// false
```

## review #2

```js
/Z/.test('z');
// false
```

## review #3

```js
/a cat/.test('that is a cat');
// true
```

## review #4

```js
/cat/.test('the cat there');
// true
```

## review #5

```js
/_/.test('happy_cat');
// true
```

## review #6

```js
/a cat/.test('that is a cat');
// true
```

## review #7

```js
/a cat/.test('that is a cat');
// true
```

## review #8

```js
/A/.test('a');
// false
```

## review #9

```js
/a/.test('b');
// false
```

## review #10

```js
/_/.test('happy_cat');
// true
```

## review #11

```js
/cat/.test('the cat there');
// true
```

## review #12

```js
/a cat/.test('that is a cat');
// true
```