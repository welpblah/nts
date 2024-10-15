# regex: maybe

- The `?` operator matches a character zero or one times, but not more than one.

```js
/^a?$/.test('a');
// true
/^a?$/.test('');
// true
/^a?$/.test('b');
// false
/^a?$/.test('aa');
// false
```

- The `?` operator affects whatever is immediately before it. For example, in `ab?`, the `?` operators only affects "b", not "a". We say that it _binds tightly_.

```js
/^ab?$/.test('a');
// true
/^ab?$/.test('b');
// false
/^ab?$/.test('ab');
// true
```

- Often, part of a string is optional. For example, US telephone numbers sometimes have area codes. But for local calls, no area code is necessary. For optional strings like this, we can use the `?` operator.

```js
/^555-?555-5555$/.test('555-555-5555');
// true
```

- Unfortunately, this regex isn't quite right. The `?` only applies to the "-" character before it, so only that character is optional. All of the other characters are required.

```js
/^555-?555-5555$/.test('555555-5555');
// true
```

- To make ? include more characters, we can group them using parentheses. Then we apply the ? to the whole group.

```js
/^(555-)?555-5555$/.test('555-555-5555');
// true
/^(555-)?555-5555$/.test('555-5555');
// true
/^(555-)?555-5555$/.test('555555-5555');
// false
```

---

## review #1

```js
/^ab?$/.test('b');
// false
```

## review #2

```js
/^ab?$/.test('b');
// false
```

## review #3

```js
/^ab?$/.test('ab');
// true
```

## review #4

```js
/^ab?$/.test('b');
// false
```

## review #5

```js
/^ab?$/.test('ab');
// true
```

## review #6

```js
/^ab?$/.test('b');
// false
```
