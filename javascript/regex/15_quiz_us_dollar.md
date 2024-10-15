# regex quiz #6

Write a regular expression to match US dollar amounts. The amounts always have two cents included, like `$9.52`.

```js
var re = /^\$\d+\.\d\d$/;
```