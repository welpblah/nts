# regex: character classes

- Regexes have many special cases to solve common problems. For example, we often write regexes to process source code. Most code contains identifiers: variable names, function names, etc. In most languages, identifiers can have letters, numbers, and underscores.

- We could write `[A-Za-z0-9_]` to mean "letters, numbers, and underscores." But that's verbose, and regexes are meant to be terse. Instead, we can use the _character class_ `\w`. The "w" in `\w` stands for "word", which is another name for an identifier. This can be tricky: "word" has a special meaning in programming!

```js
/\w/.test('a');
// true
/\w/.test('+');
// false
/\w/.test('F');
// true
/\w/.test('_');
// true
/a\wc/.test('abc');
// true
/a\wc/.test('a-c');
// false
/^\w$/.test('aaa');
// false
```

- We can match entire identifiers by combining `\w` with `+` and boundaries.

```js
/^\w+$/.test('my_variable');
// true
/^\w+$/.test('1+1');
// false
/^\w+$/.test('while');
// true
/^\w+$/.test('while (true)');
// false
```

- As with any character class, upper-casing the class negates it. (`\W` is the opposite of `\w`.)

```js
/\w/.test('c');
// true
/\W/.test('c');
// false
/\w/.test('!');
// false
/\W/.test('!');
// true
```

---

## review #1

```js
/\w/.test('+');
// false
```

## review #2

```js
/\w/.test('a');
// true
```

## review #3

```js
/^\w+$/.test('while');
// true
```

## review #4

```js
/\w/.test('_');
// true
```

## review #5

```js
/^\w+$/.test('while (true)');
// false
```

## review #6

```js
/\w/.test('+');
// false
```

## review #7

```js
/\w/.test('a');
// true
```

## review #8

```js
/\d/.test('9');
// true
```

## review #9

```js
/^\w+$/.test('while');
// true
```

## review #10

```js
/\w/.test('_');
// true
```

## review #11

```js
/^\w+$/.test('while (true)');
// false
```