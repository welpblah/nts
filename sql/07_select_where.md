# sql: select where

- Examples in this course use small tables because they're easy to work with. In production applications, tables with 1,000,000 rows are common. 1,000,000,000 rows is possible but more difficult.

(Here's our small table. Imagine it has 1,000,000 rows!)

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.j@example.com', 'Betty')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.k@example.com', 'Betty')`);
exec(`INSERT INTO users (email, name) VALUES ('cindy@example.com', 'Cindy')`);
/* [] */
```

- We don't want to retrieve an entire table of 1,000,000 users whenever Amir logs in. Instead, we query just Amir's row using `WHERE`.

- The expression `SELECT ... WHERE /* condition here */` returns only the users for whom the condition is true.

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(`SELECT * FROM users WHERE name = 'Amir'`);
/* [{email: 'amir@example.com', name: 'Amir'}] */
```

- Note that the standard equality operator in SQL is `=`, not `==` or `===`. We use `=` to compare numbers and strings. (SQLite does actually allow `==`, but that's unusual. PostgreSQL and MySQL don't allow it.)

- When we use both a `WHERE` and a `SELECT`, they can reference different columns.

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(`SELECT email FROM users WHERE name = 'Amir'`);
/* [{email: 'amir@example.com'}] */
```

- In English, that query could be written as "find the email address for the user named Amir".

- The columns named in the `WHERE` have to exist. Otherwise it's an error.

```js
exec(`SELECT email FROM users WHERE login_count = 1`);
/* Error: no such column: login_count */
```

Here's a code problem:

When multiple rows match a `WHERE`, they're all returned. Write a query to select the names of all cats whose ages are 3.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`INSERT INTO cats (name, age) VALUES ('Katy Purry', 5)`);
exec(`INSERT INTO cats (name, age) VALUES ('Wilford', 3)`);
exec(`SELECT name FROM cats WHERE age = 3`);
/* [{name: 'Ms. Fluff'}, {name: 'Wilford'}] */
```

Here's a code problem:

SQL databases support the usual comparison operators like `<` and `>`. Write a query to select the names of all cats over 4 years old.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`INSERT INTO cats (name, age) VALUES ('Katy Purry', 5)`);
exec(`INSERT INTO cats (name, age) VALUES ('Wilford', 3)`);
exec(`SELECT name FROM cats WHERE age > 4`);
/* [{name: 'Katy Purry'}] */
```

- Most SQL databases support not-equal comparisons with the familiar `!=` operator. Sometimes you'll also see the `<>` operator, as in `a <> b`, which means the same thing.

Here's a code problem:

Write a query to select the names of all cats who aren't 3 years old.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`INSERT INTO cats (name, age) VALUES ('Katy Purry', 5)`);
exec(`INSERT INTO cats (name, age) VALUES ('Wilford', 3)`);
exec(`SELECT name FROM cats WHERE age != 3`);
/* [{name: 'Keanu'}, {name: 'Katy Purry'}] */
```

- We can select by multiple columns using `AND` and `OR`. Remember that `WHERE` can match multiple rows!

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.j@example.com', 'Betty')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.k@example.com', 'Betty')`);
exec(`INSERT INTO users (email, name) VALUES ('cindy@example.com', 'Cindy')`);
/* [] */
```

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(`
  SELECT email FROM users
  WHERE name = 'Betty';
`);
/* [{email: 'betty.j@example.com'}, {email: 'betty.k@example.com'}] */
```

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(`
  SELECT email FROM users
  WHERE name = 'Betty' AND email = 'betty.j@example.com';
`);
/* [{email: 'betty.j@example.com'}] */
```

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(`
  SELECT email FROM users
  WHERE name = 'Amir' OR name = 'Cindy';
`);
/* [{email: 'amir@example.com'}, {email: 'cindy@example.com'}] */
```

Here's a code problem:

Write a query to select the ages of the cats named "Keanu" or "Katy Purry".

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`INSERT INTO cats (name, age) VALUES ('Katy Purry', 5)`);
exec(`INSERT INTO cats (name, age) VALUES ('Wilford', 3)`);
exec(`SELECT age FROM cats WHERE name = 'Keanu' OR name = 'Katy Purry'`);
/* [{age: 2}, {age: 5}] */
```

- `WHERE` clauses can call functions. For example, SQLite defines a `length` function that works on strings.

```js
exec(`CREATE TABLE users (name TEXT)`);
exec(`INSERT INTO users (name) VALUES ('Amir')`);
exec(`INSERT INTO users (name) VALUES ('Betty')`);
exec(`SELECT name FROM users WHERE length(name) > 4`);
/* [{name: 'Betty'}] */
```

- Different database systems provide different functions. SQLite only provides a few functions, but PostgreSQL [provides hundreds](https://www.postgresql.org/docs/12/functions.html). We can also define our own functions, then use them in queries. (We'll see examples of that later in this course.)

---

## review #1

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.j@example.com', 'Betty')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.k@example.com', 'Betty')`);
exec(`INSERT INTO users (email, name) VALUES ('cindy@example.com', 'Cindy')`);

exec(`SELECT email FROM users WHERE login_count = 1`);
// Error: no such column: login_count
```

## review #2

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.j@example.com', 'Betty')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.k@example.com', 'Betty')`);
exec(`INSERT INTO users (email, name) VALUES ('cindy@example.com', 'Cindy')`);

exec(`
  SELECT email FROM users
  WHERE name = 'Amir' OR name = 'Cindy';
`);
// [{email: 'amir@example.com'}, {email: 'cindy@example.com'}]
```

## review #3

Here's a code problem:

Write a query to select the ages of the cats named "Keanu" or "Katy Purry".

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`INSERT INTO cats (name, age) VALUES ('Katy Purry', 5)`);
exec(`INSERT INTO cats (name, age) VALUES ('Wilford', 3)`);
exec(`SELECT age FROM cats WHERE name = 'Keanu' OR name = 'Katy Purry'`);
// [{age: 2}, {age: 5}]
```

## review #4

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.j@example.com', 'Betty')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.k@example.com', 'Betty')`);
exec(`INSERT INTO users (email, name) VALUES ('cindy@example.com', 'Cindy')`);

exec(`SELECT email FROM users WHERE login_count = 1`);
// Error: no such column: login_count
```