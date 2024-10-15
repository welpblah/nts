# sql: comparing with null

- `NULL` in SQL databases has some sharp edges. For example, mathematical operations on `NULL` give another `NULL` (which comes back to us as JavaScript's `null`).

```js
exec(`SELECT NULL + 0 AS result`);
// [{result: null}]
exec(`SELECT NULL + 1 AS result`);
// [{result: null}]
exec(`SELECT NULL * 5 AS result`);
// [{result: null}]
```

- When we use `=` to compare anything with `NULL`, we get another `NULL`. That's even true when comparing `NULL = NULL`.

```js
exec(`SELECT NULL = NULL AS result`);
// [{result: null}]
exec(`SELECT NULL = 1 AS result`);
// [{result: null}]
exec(`SELECT 'cat' = NULL AS result`);
// [{result: null}]
```

- Fortunately, SQL also has `IS NULL` and `IS NOT NULL` comparisons that properly check for `NULL` values. (As usual, SQLite uses 1 and 0 to represent true and false.)

```sql
exec(`SELECT NULL IS NULL AS result`);
// [{result: 1}]
```

```js
exec(`SELECT NULL IS NOT NULL AS result`);
// [{result: 0}]
exec(`SELECT 5 IS NULL AS result`);
// [{result: 0}]
exec(`SELECT 5 IS NOT NULL AS result`);
// [{result: 1}]
exec(`SELECT (NULL = NULL) IS NULL AS result`);
// [{result: 1}]
```

- Usually, you'll see `IS NULL` and `IS NOT NULL` as conditions in `WHERE` queries.

- For example, suppose that we have a database where only some users have emails. We want to email all of our users. To do that, we need to select all users who have an email address.

```js
exec(`CREATE TABLE users (name TEXT NOT NULL, email TEXT NULL)`);
exec(`INSERT INTO users (name, email) VALUES ('Amir', 'amir@example.com')`);
exec(`INSERT INTO users (name, email) VALUES ('Cindy', NULL)`);
exec(`SELECT * FROM users WHERE email IS NOT NULL`);
// [{name: 'Amir', email: 'amir@example.com'}]
```

Here's a code problem:

Here's a table where some users have a cat, and some don't. Select all users that have a cat.

```js
exec(`CREATE TABLE users (name TEXT NOT NULL, cat_name TEXT NULL)`);
exec(`INSERT INTO users (name, cat_name) VALUES ('Amir', 'Ms. Fluff')`);
exec(`INSERT INTO users (name, cat_name) VALUES ('Betty', 'Keanu')`);
exec(`INSERT INTO users (name, cat_name) VALUES ('Cindy', NULL)`);
exec(`INSERT INTO users (name, cat_name) VALUES ('Dalili', NULL)`);
exec(`INSERT INTO users (name, cat_name) VALUES ('Wilford', 'Wilford')`);
exec(`SELECT * FROM users WHERE cat_name IS NOT NULL`);
// [{name: 'Amir', cat_name: 'Ms. Fluff'}, {name: 'Betty', cat_name: 'Keanu'}, {name: 'Wilford', cat_name: 'Wilford'}]
```

- The specific details of null handling vary from database to database. Fortunately, SQLite's `NULL` behavior was designed to be similar to other SQL databases, so the specifics above apply to most databases.

---

## review #1

```js
exec(`SELECT NULL + 1 AS result`);
// [{result: null}]
```

## review #2

```js
exec(`SELECT 5 IS NOT NULL AS result`);
// [{result: 1}]
```

## review #3

Here's a code problem:

Here's a table where some users have a cat, and some don't. Select all users that have a cat.

```js
exec(`CREATE TABLE users (name TEXT NOT NULL, cat_name TEXT NULL)`);
exec(`INSERT INTO users (name, cat_name) VALUES ('Amir', 'Ms. Fluff')`);
exec(`INSERT INTO users (name, cat_name) VALUES ('Betty', 'Keanu')`);
exec(`INSERT INTO users (name, cat_name) VALUES ('Cindy', NULL)`);
exec(`INSERT INTO users (name, cat_name) VALUES ('Dalili', NULL)`);
exec(`INSERT INTO users (name, cat_name) VALUES ('Wilford', 'Wilford')`);
exec(`SELECT * FROM users WHERE cat_name IS NOT NULL`);
// [{name: 'Amir', cat_name: 'Ms. Fluff'}, {name: 'Betty', cat_name: 'Keanu'}, {name: 'Wilford', cat_name: 'Wilford'}]
```

## review #4

```js
exec(`SELECT NULL + 1 AS result`);
// [{result: null}]
```