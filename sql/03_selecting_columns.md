# sql: selecting columns

- So far in this course, we've always made queries using `SELECT *`. The `*` means "all columns".

```js
exec(`CREATE TABLE users (name TEXT, login_count INTEGER)`);
exec(`INSERT INTO users (name, login_count) VALUES ('Amir', 1)`);
exec(`SELECT * FROM users`);
/* [{name: 'Amir', login_count: 1}] */
```

- Instead of the `*`, we can ask for only the columns that we care about. This is a good way to reduce the amount of data that your query transfers over the network. If a table has 20 columns, but we only need one of them, then there's no reason to request all 20 from the database.

- When we `SELECT` columns by name, only those columns are returned to us. Keep in mind that we still get an array of rows, and each row will be an object, so the result here is `[{name: /* name here */ }]`.

```js
exec(`CREATE TABLE users (name TEXT, login_count INTEGER)`);
exec(`INSERT INTO users (name, login_count) VALUES ('Amir', 1)`);
exec(`SELECT name FROM users`);
/* [{name: 'Amir'}] */
```

Here's a code problem:

Write a query that selects only the `login_count` column.

```js
exec(`CREATE TABLE users (name TEXT, login_count INTEGER)`);
exec(`INSERT INTO users (name, login_count) VALUES ('Amir', 1)`);
exec(`SELECT login_count FROM users`);

/* [{login_count: 1}]*/
```

- We can select multiple columns by separating the columns names with a comma:

```js
exec(`CREATE TABLE users (name TEXT, login_count INTEGER, age INTEGER)`);
exec(`INSERT INTO users (name, login_count, age) VALUES ('Amir', 1, 36)`);
exec(`SELECT age, name FROM users`);
/* [{age: 36, name: 'Amir'}] */
```

Here's a code problem:

Selecting a column that doesn't exist is an error. Try to select an `email` column, even though it doesn't exist.

```js
exec(`CREATE TABLE users (name TEXT, login_count INTEGER)`);
exec(`INSERT INTO users (name, login_count) VALUES ('Amir', 1)`);
exec(`SELECT email FROM users`);

/* Error: no such column: email */
```

---

## review #1

Here's a code problem:

Selecting a column that doesn't exist is an error. Try to select an `email` column, even though it doesn't exist.

```js
exec(`CREATE TABLE users (name TEXT, login_count INTEGER)`);
exec(`INSERT INTO users (name, login_count) VALUES ('Amir', 1)`);
exec(`SELECT email FROM users`);
// Error: no such column: email
```

## review #2

```js
exec(`CREATE TABLE users (name TEXT, login_count INTEGER)`);
exec(`INSERT INTO users (name, login_count) VALUES ('Amir', 1)`);
exec(`SELECT name FROM users`);
// [{name: 'Amir'}]
```