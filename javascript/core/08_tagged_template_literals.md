# core: tagged template literals

Template literal strings can be "tagged" with a function. The tag function has access to the text and template values in the string, and can modify or replace the string.

To use a tagged template literal, we write the function's name immediately followed by the template literal, like `` myTagFunction`some string here` ``..

In our first example, the tag function returns the static string `'hello'`, ignoring the template literal's value.

```js
function sayHello() {
  return 'hello';
}
sayHello`the numbers ${1} and ${1 + 1}`;
// 'hello'
```

A tagged template literal returns whatever the tag function returns. In the example above, we tagged the template literal with `sayHello`. We got back `sayHello`'s return value, which in this case is just `'hello'`. Note that there are no parentheses around the template literal string. This isn't a regular function call!

A tag function that returns a static string isn't very useful! The real power of tagged template literals is in modifying the template literal string. JavaScript splits the template literal into pieces, passing them to our tag function as arguments. Then we can build new strings, or even non-string values, out of the pieces.

To see this, we'll define a helper function `returnsItsArguments`. We've designed its signature to capture and return the arguments passed to tag functions.

```js
function returnsItsArguments(strings, ...values) {
  return {
    strings: strings,
    values: values,
  };
}
// undefined
```

We can use `returnsItsArguments` to see what arguments our tag functions get. Let's see the result; then we'll discuss it.

```js
// Note: this code example reuses elements (variables, etc.) defined in earlier examples.
returnsItsArguments`one${2}three`;
// {strings: ['one', 'three'], values: [2]}
```

The first argument is an array of the literal strings in the template literal. Literal strings are the parts that are not inside of a `${...}`. The rest parameter, `...values`, collects all of the interpolated values into an array. Interpolated values are the parts that are inside of a `${...}`.

Why are the literal strings passed in separately from the interpolated values? Why doesn't JavaScript combine them into one array for us? The reason is that it would be hard to keep track of which values are literal vs. interpolated, especially when the interpolated values are also strings.

If the template literal ends in an interpolated value, like `` `age: ${age}` ``, JavaScript will insert one more empty string, `''`, at the end.

```js
// Note: this code example reuses elements (variables, etc.) defined in earlier examples.
returnsItsArguments`one${2}`;
// {strings: ['one', ''], values: [2]}
```

Likewise, when a template literal begins with an interpolated value, like `` `${years} years old` ``, JavaScript will insert an empty string, `''`, at the beginning. Because of these empty strings at the beginning and end, `strings` always has exactly one more element than `...values` has. This will be useful in a moment!

```js
// Note: this code example reuses elements (variables, etc.) defined in earlier examples.
returnsItsArguments`${1}two`;
// {strings: ['', 'two'], values: [1]}
```

Watch out for spaces in the literal strings. In the next example, the literal strings are `['the numbers ', ' and ', '']`. The interpolated values are `[1, 2]`.

```js
// Note: this code example reuses elements (variables, etc.) defined in earlier examples.
returnsItsArguments`${1}two`;
// {strings: ['the numbers ', ' and ', ''], values: [1, 2]}
```

The tag function can do anything with the strings and interpolated values. For example, it can mimic the regular variable interpolation that happens in normal template literal strings. (That's the kind that we've already seen, where `` `1 + 1 = ${1 + 1}` `` evaluates to `'1 + 1 = 2'`.)

The next example "reassembles" the template literal string we pass in. Note the `if (i < values.length)`. It's working around the fact that there's always one final string literal with no corresponding interpolated value.

```js
function interpolate(strings, ...values) {
  let result = '';
  for (let i=0; i<strings.length; i++) {
    result += strings[i];
    if (i < values.length) {
      result += values[i];
    }
  }
  return result;
}
interpolate`the numbers ${1} and ${2}`;
// 'the numbers 1 and 2'
```

We can do more than just mimic regular template literal interpolation. We can also modify the interpolated values before inserting them into the final string.

(The solution to the next code problem is very similar to the code example above.)

Here's a code problem:

Write a `doubleNumbers` template literal tag. It mimics normal string interpolation, but doubles all of the interpolated values as they're inserted.

```js
function doubleNumbers(strings, ...values) {
  let result = '';
  for (let i=0; i<strings.length; i++) {
    result += strings[i];
    if (i < values.length) {
      result += (2 * values[i]).toString();
    }
  }
  return result;
}
doubleNumbers`the numbers ${1} and ${2}`;
// 'the numbers 2 and 4'
```

Tagged template literals have many practical applications, especially in library and framework code. For example, we can use them to escape values in HTML.

HTML tags are surrounded by angle brackets, like `<p>`. When we render data on a page written in HTML, we need to "escape" special characters like `<` and `>`. For example, we might want the string "1 < 2" to show up on a web page. But in HTML, `<` means something special: it starts a tag, like `<p>`.

If we want the string "1 < 2" on the page, we have to escape it, like `<p>1 &lt; 2</p>`. The "lt" in `&lt;` stands for "less than", the "<" character. When we view the HTML in a browser, `&lt;` shows up as `<`. But writing it as `&lt;` avoids a conflict with the HTML tag syntax.

To accomplish this, we'll build up a system that can escape "<" and ">" characters for any string. We'll write template literals like `` `<p>${user.name}</p>` ``. If the user's name contains a `'<'`, then it should be escaped to `'&lt;'`. Likewise, `'>'` should be escaped to `'&gt;'`. For example, if the user's name is `'Amir >_<'`, the final HTML-safe string should be `'<p>Amir &gt;_&lt;</p>'`.

First, we'll write a regular function that takes a string and replaces the `>` and `<` characters with the HTML-safe versions. (This `escapeOneHTMLValue` function is far from complete; please don't use it in a real system!)

```js
function escapeOneHTMLValue(value) {
  return value
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;');
}
escapeOneHTMLValue(`2<3`);
// '2&lt;3'
```

Now, we want to apply that function to any interpolated value in the template literal. That's a perfect use case for a tag function! We'll build an `escapeHTML` tag function that escapes each value in the template literal.

(The solution here will be very similar to the `doubleNumbers` function that we wrote earlier in this lesson. This time, rather than simply multiplying each value by 2, we'll call `escapeOneHTMLValue` on it.)

Here's a code problem:

Write a template tag function `escapeHTML` that takes a template literal and escapes angle brackets in all interpolated values. An `escapeOneHTMLValue` function is provided for you. You'll need to call it on each interpolated value. Remember that `strings` is always longer than `values` by exactly 1 element.

```js
function escapeOneHTMLValue(value) {
  return value
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;');
}

// code here
function escapeHTML(strings, ...values) {
  let result = '';
  for (let i=0; i<strings.length; i++) {
    result += strings[i];
    if (i < values.length) {
      result += escapeOneHTMLValue(values[i]);
    }
  }
  return result;
}
// end

const user = {
  name: 'Amir >_<',
};

escapeHTML`<p>${user.name}</p>`;
// '<p>Amir &gt;_&lt;</p>'
```

The approach above is used in the popular [common-tags](https://www.npmjs.com/package/common-tags) library's `safeHtml` tagged template literal. (That library also provides many other pre-made template tags.)

One final note about tagged template literals. If you don't use semicolons in your JavaScript, you might encounter some strange syntax problems.

```js
const obj = {a: 1}
`an object: ${obj}`;
// ReferenceError: Cannot access 'obj' before initialization
```

This error message seems wrong: `obj` is clearly defined before it's used! The problem is that JavaScript isn't parsing this code in the way we think it should. It thinks that the object `{a: 1}` is being used as a tag function for the template literal below. Here's the same thing reformatted to make it more clear:

```js
const obj = (
  ({a: 1})`an object: ${obj}`
);
// ReferenceError: Cannot access 'obj' before initialization
```

We can fix this by using semicolons all the time. Or we can insert one semicolon here so that the code parses in the way that we expect.

(The result here still isn't very useful because of the way that `toString` works on objects. But at least we got a result rather than an error!)

```js
const obj = {a: 1};
`an object: ${obj}`;
// 'an object: [object Object]'
```

---

## review

```js
function returnsItsArguments(strings, ...values) {
  return {
    strings: strings,
    values: values,
  };
}

returnsItsArguments`first${'second'}`;
// {strings: ['first', ''], values: ['second']}
```
