# regex: hex codes

- Computers internally store text as numbers. As a shorthand, we usually write those numbers out as _hexadecimal codes_.

```js
/\x41/.test('A'); // 'A' is x41
// true
```

- For example, capital letter "A" has the hex code `x41`. Regular expressions and most programming languages allow this "x" syntax. To match "A" in a regex, we can write `\x41`.

```js
/\x41/.test('an apple');
// false
/\x41/.test('CATS ARE GOOD');
// true
```

- `x42` is "B".

```js
/\x42/.test('An apple');
// false
/\x42/.test('Both apples');
// true
```

- Each _digit_ can be a number, or a letter between "a" and "f". For example, "M" is `x4d` in hex. (The `x` isn't a hex digit; it's telling us that this is a hex code.)

```js
/\x4d/.test('M');
// true
```

- There's a hex code for each character, even ones that aren't letters. "?" is `x3f` and "!" is `x21`.

```js
/\x3f/.test('?');
// true
/\x21/.test('!');
// true
```

- We can use hex codes to match symbols that you may not be able to type.

- The letter "ø" doesn't appear on an US-English keyboard. To match that letter, we can use its hex code `\xf8`.

```js
/\xf8/.test('søt katt');
// true
```

- If we have a keyboard that can type "ø", we can put it directly in a regex.

```js
/ø/.test('smørbrød');
// true
```

- Be careful: the syntax `\x` can only be followed by exactly two digits. Anything after the two digits will be a different part of the regex.

```js
/\x41d/.test('A');
// false
/\x5Ad/.test('Zd'); // "Z" is x5A
// true
```

- If we write an `\x` with only one digit, it's no longer a character code. Instead, the `\x` matches literal "x" characters.

```js
/\x4/.test('x4'); // This is probably a mistake with \x!
// true
```

- Hexadecimal digits can be any character from 0-9, a-f, or A-F. If we use the wrong characters, the `\x` will match literal "x" again.

```js
/\xfg/.test('xfg');
// true
```

---

## review #1

```js
/\x41/.test('A'); // 'A' is x41
// true
```

## review #2

```js
/ø/.test('smørbrød');
// true
```

## review #3

```js
/\x4/.test('x4'); // This is probably a mistake with \x!
// true
```

## review #4

```js
/\x5Ad/.test('Zd'); // "Z" is x5A
// true
```

## review #5

```js
/\x4/.test('x4'); // This is probably a mistake with \x!
// true
```

## review #6

```js
/\xfg/.test('xfg');
// true
```

## review #7

```js
/\x4/.test('x4'); // This is probably a mistake with \x!
// true
```

## review #8

```js
/\x41/.test('A'); // 'A' is x41
// true
```

## review #9

```js
/ø/.test('smørbrød');
// true
```

## review #10

```js
/\xfg/.test('xfg');
// true
```

## review #11

```js
/\x5Ad/.test('Zd'); // "Z" is x5A
// true
```

## review #12

```js
/\x4/.test('x4'); // This is probably a mistake with \x!
// true
```