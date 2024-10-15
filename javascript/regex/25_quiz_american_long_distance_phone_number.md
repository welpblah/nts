# regex quiz #11

American phone numbers can be local or long-distance. A local number looks like 555-1234. A long-distance number adds an area code, like 206-555-1234. Write a regex that can recognize both types.

```js
var re = /^(\d\d\d-)?\d\d\d-\d\d\d\d$/;
```