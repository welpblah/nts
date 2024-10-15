# regex: character sets

- In a character set, characters and ranges can be mixed in any order. This regex is equivalent to `/[g]|[c-e]|[a]/`.

```js
/[gc-ea]/.test('a');
// true
/[gc-ea]/.test('b');
// false
/[gc-ea]/.test('c');
// true
/[gc-ea]/.test('d');
// true
/[gc-ea]/.test('h');
// false
```

- We can also negate the whole character set, even if it's complex.

```js
/[^gc-ea]/.test('a');
// false
/[^gc-ea]/.test('d');
// false
/[^gc-ea]/.test('b');
// true
```

- If a character set ever gives you trouble, you can always break it up. For example, `/[hbd-fa]/` can be thought of as `/(h|b|[d-f]|a)/`. This trick works for anything in regexes. Most regex features are _syntactic sugar_ for simple features like `|`.

- Special characters aren't special inside a _character set_. For example, "." means a literal "." and "$" is a literal "$".

```js
/[a$]/.test('$');
// true
/[a$]/.test('a');
// true
/[a$]/.test('^');
// false
/^[a$]$/.test('ab');
// false
```

- The `^` is only special if it's the first character in the set. There, it means "negate this set". But a `^` anywhere else in the set is just another literal character.

```js
/[^b]/.test('b');
// false
/[b^]/.test('b');
// true
/[b^]/.test('^');
// true
// true
/[^^]/.test('^');
// false
```

---

## review #1

```js
/[^gc-ea]/.test('d');
// false
```

## review #2

```js
/[gc-ea]/.test('d');
// true
```

## review #3

```js
/[b^]/.test('^');
// true
```

## review #4

```js
/[^^]/.test('^');
// false
```

## review #5

```js
/[gc-ea]/.test('d');
// true
```

## review #6

```js
/[gc-ea]/.test('d');
// true
```

## review #7

```js
/[^gc-ea]/.test('d');
// false
```

## review #8

```js
/[b^]/.test('^');
// true
```

## review #9

```js
/[^^]/.test('^');
// false
```

## review #10

```js
/[^^]/.test('^');
// false
```

## review #11

```js
/[gc-ea]/.test('d');
// true
```

## review #12

```js
/[^^]/.test('^');
// false
```