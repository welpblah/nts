# sql: null in unique constraints

- What happens if there are `NULL`s in a column with a `UNIQUE` constraint? At first glance, here's what you might expect: one `NULL` is allowed, but multiple `NULL`s will violate the `UNIQUE` constraint. But that would make some things very difficult in practice. Here's an example:

- Suppose that we have a `users` table with an `email` column, which has a `UNIQUE` constraint. Some users will register with a third-party authentication system like Google's, Twitter's, or GitHub's. Those users will have a `NULL` email address column.

- If a `UNIQUE` constraint only allowed one `NULL`, then only one user would be allowed to register with those third-party authentication systems. After that, all further registration attempts would violate the `UNIQUE` constraint.

- For exactly this reason, `UNIQUE` has special behavior for `NULL`. `NULL` values are effectively ignored by a `UNIQUE` constraint.

- (In the following examples, you can answer with `error` if a query will result in an error. Queries like `INSERT` and `CREATE` that return no rows will have a return value of `[]`.)

```js
exec(`CREATE TABLE users (email TEXT NULL UNIQUE)`);
exec(`INSERT INTO users (email) VALUES ('amir@example.com')`);
exec(`INSERT INTO users (email) VALUES ('betty@example.com')`);
// []
```

```js
exec(`CREATE TABLE users (email TEXT NULL UNIQUE)`);
exec(`INSERT INTO users (email) VALUES (NULL)`);
exec(`SELECT * FROM users`);
// [{email: null}]
```

```js
exec(`CREATE TABLE users (email TEXT NULL UNIQUE)`);
exec(`INSERT INTO users (email) VALUES ('amir@example.com')`);
exec(`INSERT INTO users (email) VALUES ('amir@example.com')`);
// Error: UNIQUE constraint failed: users.email
```

```js
exec(`CREATE TABLE users (email TEXT NULL UNIQUE)`);
exec(`INSERT INTO users (email) VALUES (NULL)`);
exec(`INSERT INTO users (email) VALUES (NULL)`);
exec(`SELECT * FROM users`);
// [{email: null}, {email: null}]
```

- Sometimes, language quirks are mistakes: a language designer might not anticipate problems with the language they've designed. This `NULL`/`UNIQUE` behavior is a quirk, but it's not a language design mistake. It's an intentional choice that allows us to build databases like the one above.

---

## review #1

```js
exec(`CREATE TABLE users (email TEXT NULL UNIQUE)`);
exec(`INSERT INTO users (email) VALUES ('amir@example.com')`);
exec(`INSERT INTO users (email) VALUES ('amir@example.com')`);
// Error: UNIQUE constraint failed: users.email
```

## review #2

```js
exec(`CREATE TABLE users (email TEXT NULL UNIQUE)`);
exec(`INSERT INTO users (email) VALUES (NULL)`);
exec(`INSERT INTO users (email) VALUES (NULL)`);
exec(`SELECT * FROM users`);
// [{email: null}, {email: null}]
```

