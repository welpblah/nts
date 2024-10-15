# regex: word boundaries

- When using a regular expression to search for a word, we usually don't want to match those letters inside another word.

```js
/cat/.test("I couldn't locate Mr. Meow.");
// true
```

- The word 'locate' contains 'cat', so this sentence matches.

- That's not what we want! We'd like to match the word 'cat'. Regexes provide a way to do this using the _word boundary_ `\b`:

```js
/\bcat\b/.test('It was difficult to locate, but');
// false
/\bcat\b/.test('the cat returned.');
// true
```

- `\b` only matches where a word character is next to a non-word character. (Remember that word-characters are letters, numbers and underscores.)

```js
/\bcat\b/.test('cat-like reflexes');
// true
/\bcat\b/.test("var cat_name = 'Mr. Meow';");
// false
/\bcat\b/.test("Where's the cat's toy?");
// true
```

- Like most character classes, `\b` can be _negated_ by capitalizing it. `\B` only matches between two word characters. It's pronounced "non-word-boundary".

```js
/\Bcat\B/.test('The cat over there');
// false
/\Bcat\B/.test('concatenate');
// true
```

- `\B` can be used to find out if a word has 'cat' in particular places, which could help win Scrabble games.

- If you'd like to find words that contain "cat", but don't end with "cat":

```js
/cat\B/.test('publication');
// true
/cat\B/.test('wildcat');
// false
/cat\B/.test('catenary');
// true
```

- Or words that contain "cat" only in the middle:

```js
/\Bcat\B/.test('cathode');
false
/\Bcat\B/.test('muscat');
// false
/\Bcat\B/.test('hecatomb');
// true
```

---

## review #1

```js
/cat/.test("I couldn't locate Mr. Meow.");
// true
```

## review #2

```js
/\bcat\b/.test('cat-like reflexes');
// true
```

## review #3

```js
/\bcat\b/.test("var cat_name = 'Mr. Meow';");
// false
```

## review #4

```js
/\bcat\b/.test("var cat_name = 'Mr. Meow';");
// false
```

## review #5

```js
/\bcat\b/.test("var cat_name = 'Mr. Meow';");
// false
```

## review #6

```js
/cat/.test("I couldn't locate Mr. Meow.");
// true
```

## review #7

```js
/\bcat\b/.test('cat-like reflexes');
// true
```

## review #8

```js
/\bcat\b/.test('cat-like reflexes');
// true
```

## review #9

```js
/\bcat\b/.test("var cat_name = 'Mr. Meow';");
// false
```