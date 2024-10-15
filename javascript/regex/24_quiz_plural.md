# regex quiz #10

Write a regex to match only 'cat' or 'cats'. If there's anything more or less than that, the regex shouldn't match.

```js
var re = /(^cat$)|(^cats$)/;
```

Alternatively:

```js
var re = /^cats?$/;
```