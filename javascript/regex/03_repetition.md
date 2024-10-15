# regex: repetition

- The `+` _operator_ requires something to occur one or more times.

```js
/a+/.test('aaa');
// true
/a+/.test('a');
// true
/a+/.test('');
// false
/cat/.test('caaat');
// false
/ca+t/.test('caaat');
// true
/ca+t/.test('ct');
// false
```

- `+` works with `.`, just like it does with _literal_ characters.

```js
/.+/.test('a');
// true
/.+/.test('cat');
// true
/.+/.test('');
// false
/a.+z/.test('aveloz');
// true
/a.+z/.test('az');
// false
```

- The `*` operator is similar to `+`, but means "zero or more times".

```js
/a*/.test('a');
// true
/a*/.test('aa');
// true
/a*/.test('');
// true
/a.*z/.test('aveloz');
// true
/a.*z/.test('az');
// true
```

- We can use multiple `+` and `*`s in the same regex.

```js
/a+b*c+/.test('aacc');
// true
/a+b*c+/.test('aa');
// false
/a+b*c+/.test('aabccc');
// true
/a+b*c+/.test('abbbbc');
// true
/a+b*c+/.test('bc');
// false
```

---

## review #1

```js
/.+/.test('any random string');
// true
```

## review #2

```js
/ca+t/.test('ct');
// false
```

## review #3

```js
/horse+/.test('horseeeeee');
// true
```

## review #4

```js
/.+/.test('');
// false
```

## review #5

```js
/a.+z/.test('aveloz');
// true
```

## review #6

```js
/a.+z/.test('az');
// false
```

## review #7

```js
/a+b*c+/.test('aabccc');
// true
```

## review #8

```js
/a+b*c+/.test('bc');
// false
```

## review #9

```js
/a*/.test('');
// true
```

## review #10

```js
/ca+t/.test('ct');
// false
```

## review #11

```js
/.+/.test('cat');
// true
```

## review #12

```js
/ca+t/.test('caaat');
// true
```

## review #13

```js
/a.+z/.test('az');
// false
```

## review #14

```js
/.+/.test('');
// false
```

## review #15

```js
/a.+z/.test('aveloz');
// true
```

## review #16

```js
/a+b*c+/.test('aabccc');
// true
```

## review #17

```js
/a*/.test('');
// true
```

## review #18

```js
/a+b*c+/.test('bc');
// false
```

## review #19

```js
/a.+z/.test('az');
// false
```

## review #20

```js
/a.+z/.test('az');
// false
```

## review #21

```js
/a.+z/.test('az');
// false
```

## review #22

```js
/a.+z/.test('az');
// true
```
