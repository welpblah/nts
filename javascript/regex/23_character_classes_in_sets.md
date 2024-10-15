 # regex: character classes in sets

- Suppose that we're writing a regex to process Lisp code. Unlike most languages, Lisp allows the "-" character in identifiers. We can use a character set including everything in `\w` as well as `-`.

```js
/^[\w-]+$/.test('a_function');
// true
/^[\w-]+$/.test('a-function');
// true
/^[\w-]+$/.test('(describe-number)');
// false
```

---

## review #1

```js
/^[\w-]+$/.test('a-function');
// true
```

## review #2

```js
/^[\w-]+$/.test('(describe-number)');
// false
```

## review #3

```js
/^[\w-]+$/.test('(describe-number)');
// false
```

## review #4

```js
/^[\w-]+$/.test('(describe-number)');
// false
```

## review #5

```js
/^[\w-]+$/.test('(describe-number)');
// false
```

## review #6

```js
/^[\w-]+$/.test('a-function');
// true
```

## review #7

```js
/^[\w-]+$/.test('(describe-number)');
// false
```

## review #8

```js
/^[\w-]+$/.test('(describe-number)');
// false
```