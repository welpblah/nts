## regex: parens

- This regex seems to match strings of exactly one "a" or exactly one "b". But that's not all it matches!

```js
/^a|b$/.test('a');
// true
/^a|b$/.test('b');
// true
/^a|b$/.test('xa');
// false
/^a|b$/.test('ax');
// true
```

- Why did `/^a|b$/` accept the string "ax"? It makes more sense if we parenthesize the regex. (Parentheses group operators together, like in other programming languages.)

```js
/(^a)|(b$)/.test('xa');
// false
/(^a)|(b$)/.test('ax');
// true
```

- When we say `/^a|b$/`, it means the same as `/(^a)|(b$)/`. We might prefer it to mean `/^(a|b)$/`, but it is the way it is. Fortunately, adding those parentheses ourselves will help.

```js
/^(a|b)$/.test('a');
// true
/^(a|b)$/.test('xa');
// false
/^(a|b)$/.test('ax');
// false
/^(a|b)$/.test('xb');
// false
/^(a|b)$/.test('b');
// true
/^(a|b)$/.test('bx');
// false
```

- Imagine that we have a system that can read images. But it can only read JPGs, PNGs, and PDFs; anything else makes it crash. We want a regex that will accept images based on their filename.

```js
/^(jpg|png|pdf)$/.test('jpg');
// true
/^(jpg|png|pdf)$/.test('jpeg');
// false
/^(jpg|png|pdf)$/.test('png');
// true
/^(jpg|png|pdf)$/.test('pdf');
// true
```

- As in math or normal programming, parentheses can "factor out" common elements.

```sql
/^(jpg|p(ng|df))$/.test('jpg');
// true
/^(jpg|p(ng|df))$/.test('jpeg');
// false
/^(jpg|p(ng|df))$/.test('pdf');
// true
/^(jpg|p(ng|df))$/.test('png');
// true
/^(jpg|p(ng|df))$/.test('p');
// false
```

- We can use operators on parenthesized groups. In this example, the `+` matches strings of one or more characters. The `(a|b)` requires each of those characters to be an "a" or a "b".

```js
/^(a|b)+$/.test('aaaaab');
// true
/^(a|b)+$/.test('bababa');
// true
/^(a|b)+$/.test('');
// false
```

- Either side of a `|` can be empty. This regex means "exactly the letter a, or the empty string".

```js
/^(a|)$/.test('a');
// true
/^(a|)$/.test('');
// true
/^(a|)$/.test('aa');
// false
```

---

## review #1

```js
/^(a|b)$/.test('b');
// true
```

## review #2

```js
/(^a)|(b$)/.test('ax');
// true
```

## review #3

```js
/^(a|b)$/.test('bx');
// false
```

## review #4

```js
/^(jpg|png|pdf)$/.test('pdf');
// true
```

## review #5

```js
/^(jpg|p(ng|df))$/.test('png');
// true
```

## review #6

```js
/^(a|b)+$/.test('bababa');
// true
```

## review #7

```js
/^(a|b)$/.test('b');
// true
```

## review #8

```js
/(^a)|(b$)/.test('ax');
// true
```

## review #9

```js
/^(jpg|png|pdf)$/.test('pdf');
// true
```

## review #10

```js
/^(a|b)$/.test('bx');
// false
```

## review #11

```js
/^(jpg|p(ng|df))$/.test('png');
// true
```

## review #12

```js
/(^a)|(b$)/.test('ax');
// true
```

## review #13

```js
/(^a)|(b$)/.test('ax');
// true
```

## review #14

```js
/^(a|b)+$/.test('bababa');
// true
```

## review #15

```js
/^(a|b)+$/.test('bababa');
// true
```

## review #16

```js
/(^a)|(b$)/.test('ax');
// true
```

## review #17

```js
/^(a|b)+$/.test('bababa');
// true
```

```js
/(^a)|(b$)/.test('ax');
// true
```