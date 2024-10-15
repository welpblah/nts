# sql: selecting expressions

- We can `SELECT` many kinds of expressions: columns, like we've seen already; but also constants, mathematical expressions, and function calls. For example, if we `SELECT` 1, we get 1 back. If we `SELECT 1 + 1`, we get 2 back.

```js
exec(`SELECT 1`);
/* [{'1': 1}] */
```

```js
exec(`SELECT 1 + 1`);
/* [{'1 + 1': 2}] */
```

- The output here is the same as ever: an array of objects. But now, there are no column names. Instead, the properties on the "row" object are the expressions that we queried.

```js
exec(`SELECT 2 * 3`);
/* [{'2 * 3': 6}] */
```

- We can also query functions that return values dynamically. For example, the current date is written as `DATE('now')` in SQLite. This query asks: is the current date after `DATE(0)`, which was January 1, 1970?

```js
exec(`SELECT DATE('now') > DATE(0)`);
/* [{"DATE('now') > DATE(0)": 1}] */
```

Here's a code problem:

Select the number 17.

```js
exec(`SELECT 17`);
/* [{'17': 17}] */
```

---

## review #1

```js
exec(`SELECT 2 * 3`);
// [{'2 * 3': 6}]
```

## review #2

```js
exec(`SELECT 2 * 3`);
// [{'2 * 3': 6}]
```