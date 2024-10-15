# regex: wildcard

- Regexes like /a/ are _literal_: they specify exact characters to match. The real power in regexes is in the various _operators_. The most basic is `.`, the _wildcard_ operator. It matches any character. But the character must be present; `.` won't match the empty string.

```js
/./.test('a');
// true
/./.test('b');
// true
/./.test('>');
// true
/./.test('');
// false
```

- There's one important special case for `/./`: it doesn't match newlines!

```js
/./.test('\n');
// false
```

- Putting `.` next to another character means that they occur consecutively. For example, `/a./` matches an "a" followed by any character. And `/.a/` matches any character followed by an "a".

```js
/x.z/.test('xyz');
// true
/x.z/.test('xaz');
// true
/x.z/.test('xyyz');
// false
/x.z/.test('x_z');
// true
```

- When there are multiple `.`s, they can match different characters.

```js
/x..z/.test('xaaz');
// true
/x..z/.test('xaz');
// false
/x..z/.test('xabz');
// true
```

---

## review #1

```js
/./.test('&');
// true
```

## review #2

```js
/../.test('');
// false
```

## review #3

```js
/./.test('\n');
// false
```

## review #4

```js
/x.z/.test('x_z');
// true
```

## review #5

```js
/./.test('\n');
// false
```

## review #6

```js
/./.test('\n');
// false
```

## review #7

```js
/./.test('');
// false
```

## review #8

```js
/./.test('>');
// true
```

## review 9

```js
/x.z/.test('x_z');
// true
```

## review #10

```js
/x.z/.test('x_z');
// true
```

## review #11

```js
/./.test('\n');
// false
```

## review #12

```js
/x.z/.test('x_z');
// true
```
