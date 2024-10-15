# sql: updating rows

- SQL has an `UPDATE` statement for updating rows that already exist. We tell it which columns to change with `SET`. By default, it will update every row in the entire table.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`UPDATE cats SET name = 'Cat'`);
exec(`SELECT name FROM cats`);
/* [{name: 'Cat'}, {name: 'Cat'}] */
```

- We rarely want to change every row at once. To update only certain records, we can add a `WHERE`. This is the same `WHERE` clause that we use in `SELECT`s, so it can include mathematical expressions, function calls, etc.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`UPDATE cats SET name = 'Mr. Reeves' WHERE name = 'Keanu'`);
exec(`SELECT name FROM cats`);
/* [{name: 'Ms. Fluff'}, {name: 'Mr. Reeves'}] */
```

Here's a code problem:

Ms. Fluff had a birthday! 🎉 Update her age to be 4 instead of 3.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`UPDATE cats SET age = 4 WHERE name = 'Ms. Fluff'`);
exec(`SELECT * FROM cats`);
/* [{name: 'Ms. Fluff', age: 4}, {name: 'Keanu', age: 2}] */
```

- If our `UPDATE`'s `WHERE` clause matches multiple rows, then all of those rows will be updated. This makes `UPDATE` potentially dangerous. Be very careful that your `UPDATE` only affects the rows that you expected it to update!

Here's a code problem:

We have a table of rectangles, but it has incorrect data in it. We've accidentally labeled all of the rectangles as "tall" (height is greater than width). But some of them are actually "wide" (height is less than width). Write a single `UPDATE` that sets the wide rectangles' `kind`s to "wide".

```js
exec(`CREATE TABLE rects (kind TEXT, width REAL, height REAL)`);
exec(`INSERT INTO rects (kind, width, height) VALUES ('tall', 1.1, 2.7)`);
exec(`INSERT INTO rects (kind, width, height) VALUES ('tall', 4.4, 4.3)`);
exec(`INSERT INTO rects (kind, width, height) VALUES ('tall', 0.4, 8.9)`);
exec(`INSERT INTO rects (kind, width, height) VALUES ('tall', 100, 0.1)`);
exec(`UPDATE rects SET kind = 'wide' WHERE width > height`);
exec(`SELECT * FROM rects`);
/* [{kind: 'tall', width: 1.1, height: 2.7}, {kind: 'wide', width: 4.4, height: 4.3}, {kind: 'tall', width: 0.4, height: 8.9}, {kind: 'wide', width: 100, height: 0.1}] */
```

---

## review #1

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`UPDATE cats SET name = 'Cat'`);
exec(`SELECT name FROM cats`);
// [{name: 'Cat'}, {name: 'Cat'}]
```

## review #2

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`UPDATE cats SET age = 4 WHERE name = 'Ms. Fluff'`);
exec(`SELECT * FROM cats`);
// [{name: 'Ms. Fluff', age: 4}, {name: 'Keanu', age: 2}]
```

## review #3

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`UPDATE cats SET name = 'Cat'`);
exec(`SELECT name FROM cats`);
// [{name: 'Cat'}, {name: 'Cat'}]
```
