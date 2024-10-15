# regex quiz #12

Write a regex that matches valid regex hex codes, like `x2a`. When finished, your regex will recognize a small part of regex syntax. Use constrained repetition (with `{` and `}`) to match the two hex digits.

```js
var re = /^\x[a-f|A-F|0-9]{2}$/;
```

Alternatively:

```js
var re = /^x[0-9a-f]{2}$/;
```