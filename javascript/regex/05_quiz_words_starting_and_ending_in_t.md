# regex quiz #1

Write a regex that recognizes words that begin and end in "t". (The "t" at the beginning and end of the word must be separate, so the regex should match "tt" but not "t".)

```js
var re = /^t.*t$/;
```