# regex quiz #8

Define a regular expression that recognizes American local phone numbers. (They have three numbers before the dash and four after it.)

```js
var re = /^\d{3}-\d{4}$/;
```

Alternatively:

```js
var re = /^\d\d\d-\d\d\d\d$/;
```