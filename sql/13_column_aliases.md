# sql: column aliases

- We can rename columns when needed using `AS`. The result object's properties will use our column aliases instead of the original column names. In this example, we rename the `age` column, but we leave the `name` alone.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`SELECT name, age AS oldness FROM cats`);
// [{name: 'Keanu', oldness: 2}]
```

- Column aliases become useful in complex queries that involve multiple tables. However, they're also useful for something much simpler: querying mathematical expressions and functions. Now we can clean up the results of those queries!

```js
exec(`SELECT 1 + 1`);
// [{'1 + 1': 2}]
exec(`SELECT 1 + 1 AS sum`);
// [{sum: 2}]
```

- This lets us avoid retyping complicated expressions:

```js
exec(`SELECT DATE('now') > DATE(0)`);
// [{"DATE('now') > DATE(0)": 1}]
```

```js
exec(`SELECT DATE('now') > DATE(0) AS does_the_clock_work`);
// [{does_the_clock_work: 1}]
```

Here's a code problem:

Write a query to retrieve all of the cats. Alias the "name" column to "cat_name" and alias "age" to "cat_age".

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`SELECT name AS cat_name, age AS cat_age FROM cats`);
// [{cat_name: 'Keanu', cat_age: 2}]
```

---

## review #1

```js
exec(`SELECT 1 + 1 AS sum`);
// [{sum: 2}]
```

## review #2

Here's a code problem:

Write a query to retrieve all of the cats. Alias the "name" column to "cat_name" and alias "age" to "cat_age".

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`SELECT name AS cat_name, age AS cat_age FROM cats`);
// [{cat_name: 'Keanu', cat_age: 2}]
```

## review #3

```js
exec(`SELECT 1 + 1 AS sum`);
// [{sum: 2}]
```