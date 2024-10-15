# regex: escaping

- In most programming languages, strings are delimited with "double quotes". To put a literal double quote in a string, we _escape_ it. The string `"\""` contains exactly one double quote. (It's the same as `'"'`.)

- In regexes, `+` normally repeats whatever comes before it. But we can _escape_ the `+` as `\+` to match a literal "+". Likewise for other operators like `.`.

```js
/\./.test('That is a cat.');
// true
/\./.test('Is that a cat?');
// false
/.\+./.test('111');
// false
/.\+./.test('1+1');
// true
/\++/.test('+');
// true
/\++/.test('++');
// true
/\+\+/.test('++');
// true
/\+\+/.test('+');
// false
```

- Escaping can be used when you need a literal `^`, `$`, etc. (It works for any operator.)

```js
/\^/.test('^');
// true
/\$/.test('$');
// true
/\$$/.test('$a');
// false
/\$$/.test('a$');
// true
/a|b/.test('|');
// false
/a\|b/.test('a|b');
// true
```

---

## review #1

```js
/\./.test('The ! is an exclamation character!');
// false
```

## review #2

```sql
/\./.test('The . is a period character!');
// true
```

## review #3

```js
/\+\+/.test('++');
// true
```

## review #4

```js
/\+\+/.test('+');
// false
```

## review #5

```js
/\./.test('That is a cat.');
// true
```

## review #6

```js
/a\|b/.test('a|b');
// true
```

## review #7

```js
/\./.test('That is a cat.');
// true
```

## review #8

```js
/\./.test('Is that a cat?');
// false
```

## review #9

```js
/\+\+/.test('++');
// true
```

## review #10

```js
/\+\+/.test('+');
// false
```

## review #11

```js
/a\|b/.test('a|b');
// true
```

## review #12

```js
/\./.test('That is a cat.');
// true
```