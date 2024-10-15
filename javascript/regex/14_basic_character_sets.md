# regex: basic character sets

- With "or" expressions, we can recognize a whole set of characters.

```js
/^c(a|o|u)t$/.test('cat');
// true
```

- This gets tiresome if we need many options in the "or". Fortunately, we can use a _character set_ to simplify it. The set `[aou]` is equivalent to `(a|o|u)`.

```js
/^c[aou]t$/.test('cat');
// true
/^c[aou]t$/.test('cot');
// true
/^c[aou]t$/.test('cet');
// false
```

- What if we want to allow any string of lower case letters? We'd have to write `/(a|b|c|d|e|` and so on. Instead, we can write another _character set_.

```js
/[abcdefghijklmnopqrstuvwxyz]/.test('a');
// true
/[abcdefghijklmnopqrstuvwxyz]/.test('g');
// true
```

- That was shorter, but still wordy. We can specify an entire range of characters by using `-`.

```js
/[a-z]/.test('g');
// true
/[1-3]/.test('1');
// true
/[1-3]/.test('a');
// false
/[1-3]/.test('2');
// true
```

- As usual, we escape special characters when we want them to be literal. This range contains only one character, an escaped `]` written as `\]`.

```js
/[\]]/.test(']');
// true
```

- Character sets can be _negated_ to mean "everything not in the set".

- We negate with `^`, a character that we already saw. Normally it means "beginning of line". But inside [square brackets], it means "negate the character set". (There are only so many symbols on a keyboard, so some get reused.)

```js
/[^a]/.test('a');
// false
/[^a]/.test('5');
// true
```

- Negation applies to the entire character set. The regex `/[^ab]/` means "any character other than a or b".

```js
/[^ab]/.test('a');
// false
/[^ab]/.test('c');
// true
/[^ab]/.test('_');
// true
```

- Negation also applies to ranges.

```js
/[a-z]/.test('h');
// true
/[^a-z]/.test('h');
// false
/[^a-z]/.test('5');
// true
```

- Character sets match exactly one character in the string. (This is like character classes, which also match only one character.) To match more than one character, we can use `+` or `*`.

```js
/^[a-z]$/.test('cat');
// false
/^[a-z]+$/.test('cat');
// true
```

---

## review #1

```js
/[1-3]/.test('2');
// true
```

## review #2

```js
/^c[aou]t$/.test('cot');
// true
```

## review #3

```js
/[\]]/.test(']');
// true
```

## review #4

```js
/[^a-z]/.test('5');
// true
```

## review #5

```js
/^[a-z]$/.test('cat');
// false
```

## review #6

```js
/[^ab]/.test('c');
// true
```

## review #7

```js
/[^a-z]/.test('5');
// true
```

## review #8

```js
/^[a-z]+$/.test('cat');
// true
```

## review #9

```js
/^[a-z]$/.test('cat');
// false
```

## review #10

```js
/[^ab]/.test('c');
// true
```

## review #11

```js
/^[a-z]$/.test('cat');
// false
```

## review #12

```js
/[^a-z]/.test('5');
// true
```

## review #13

```js
/[^ab]/.test('c');
// true
```

## review #14

```js
/[\]]/.test(']');
// true
```

## review #15

```js
/[1-3]/.test('2');
// true
```

## review #16

```js
/^c[aou]t$/.test('cot');
// true
```

## review #17

```js
/^[a-z]+$/.test('cat');
// true
```

## review #18

```js
/^[a-z]$/.test('cat');
// false
```