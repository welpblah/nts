# regex: constrained repetition

- Sometimes we need text to be of a certain length. We could repeat `.` to enforce length.

```js
/^.....$/.test('1234');
/* false */
```

```js
/^.....$/.test('12345');
/* true */
```

```js
/^.....$/.test('123456');
/* false */
```

- This is awkward, especially if we want to match exactly, say, 20 characters. Fortunately, there's a better way: {curly braces}.

```js
/^.{5}$/.test('1234');
/* false */
```

```js
/^.{5}$/.test('12345');
/* true */
```

```js
/^.{5}$/.test('123456');
/* false */
```

- We can repeat anything in this way, not just `.`

```js
/^(a|b){3}$/.test('aaa');
/* true */
```

```js
/^(a|b){3}$/.test('bba');
/* true */
```

```js
/^(a|b){3}$/.test('ab');
/* false */
```

- By adding a comma, we can specify a range of allowed lengths.

```js
/^.{2,3}$/.test('1');
/* false */
```

```js
/^.{2,3}$/.test('12');
/* true */
```

```js
/^.{2,3}$/.test('123');
/* true */
```

```sql
/^.{2,3}$/.test('1234');
/* false */
```

- We can also specify "n or more characters" by omitting the second number. For example, `.{8,}` means "at least eight characters".

```js
/^[fho]{3,}$/.test('of');
/* false */
```

```js
/^[fho]{3,}$/.test('off');
/* true */
```

```js
/^[fho]{3,}$/.test('hoof');
/* true */
```

- In some regex systems, `.{,5}` means "at most five characters". Unfortunately, that's not true in JavaScript's regexes. JavaScript won't tell us about our mistake either. Instead, the `{,5}` gets interpreted as a literal string!

```js
/^.{,5}$/.test('12345');
/* false */
```

```js
// Watch out: JavaScript regexes are weird in this case!
/^.{,5}$/.test('.{,5}');
// true
```

- This is bizarre, but not a big problem. If we need five or fewer characters, we can say `.{0,5}`.

```js
/^.{0,5}$/.test('1234');
// true
/^.{0,5}$/.test('12345');
// true
/^.{0,5}$/.test('123456');
// false
```

## review #1

```js
/^.{5}$/.test('123456');
// false
```

## review #2

```js
/^(a|b){3}$/.test('bba');
// true
```

## review #3

```js
/^.{2,3}$/.test('12');
// true
```

## review #4
```js
/^.{5}$/.test('123456');
// false
```

## review #5

```js
/^.{,5}$/.test('.{,5}');
// true
```

## review #6

```js
/^[fho]{3,}$/.test('hoof');
// true
```

## review #7

```js
/^.{5}$/.test('123456');
// false
```

## review #8

```js
/^.{0,5}$/.test('1234');
// true
```

## review #9

```js
/^.{0,5}$/.test('12345');
// true
```

## review #10

```js
/^.{5}$/.test('123456');
// false
```

## review #11

```js
/^(a|b){3}$/.test('bba');
// true
```

## review #12

```js
/^.{2,3}$/.test('12');
// true
```

## review #13

```js
/^[fho]{3,}$/.test('hoof');
// true
```

## review #14

```js
/^.{,5}$/.test('.{,5}');
// true
```

## review #15

```js
/^.{0,5}$/.test('1234');
// true
```

## review #16

```js
/^.{0,5}$/.test('12345');
// true
```

## review #7

```js
/^.{5}$/.test('123456');
// false
```