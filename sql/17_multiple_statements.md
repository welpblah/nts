# sql: multiple statements

- SQL allows us to separate statements with `;`. When we do that, only the data from the final statement will be returned.

```js
exec(`SELECT 1; SELECT 2 AS two`);
// [{two: 2}]
```

Here's a code problem:

Use SQL's `;` syntax to:

1. Create a `users` table with a text `name`.
2. Insert a user named "Amir".
3. Select the user back out.

```js
exec(`
  CREATE TABLE users (name TEXT);
  INSERT INTO users (name) VALUES ('Amir');
  SELECT * FROM users;
`);
// [{name: 'Amir'}]
```

- When using multiple statements, later statements will always see changes made by earlier statements. That's true for `INSERT`, `UPDATE`, `DELETE`, and any other kind of change.

Here's a code problem:

Use `;` to update Amir's name to "Amir A", then select all of the users.

```js
exec(`CREATE TABLE users (name TEXT)`);
exec(`INSERT INTO users (name) VALUES ('Amir')`);
exec(`
  UPDATE users SET name = 'Amir A' WHERE name = 'Amir';
  SELECT * FROM users
`);
// [{name: 'Amir A'}]
```

- Many database APIs don't let us use `;` with bind parameters. Our database API has that constraint: if we use both of these API features at the same time, it will cause an error. Unlike most errors in this course, this one comes from Execute Program itself, not from SQLite.

```js
// Bind parameters can't be used with multiple statements.
// (Many database libraries have this limitation.)
exec(`SELECT 1; SELECT ? AS two`, [1]);
// Error: It looks like you tried to execute multiple statements with ";" while also using bind parameters. Many database APIs, including Execute Program's, don't allow you to do both of those at the same time. Try executing each statement in a separate call to "exec" instead of using semicolons. (This error is specific to Execute Program; it doesn't come from SQLite.)
```

```js
exec(`SELECT ?; SELECT ? AS two`, [1, 2]);
// Error: It looks like you tried to execute multiple statements with ";" while also using bind parameters. Many database APIs, including Execute Program's, don't allow you to do both of those at the same time. Try executing each statement in a separate call to "exec" instead of using semicolons. (This error is specific to Execute Program; it doesn't come from SQLite.)
```

---

## review #1

Here's a code problem:

Use SQL's `;` syntax to:

1. Create a `users` table with a text `name`.
2. Insert a user named "Amir".
3. Select the user back out.

```js
exec(`
  CREATE TABLE users (name TEXT NOT NULL);
  INSERT INTO users (name) VALUES ('Amir');
  SELECT * FROM users
`);
// [{name: 'Amir'}]
```

## review #2

```js
// Bind parameters can't be used with multiple statements.
// (Many database libraries have this limitation.)
exec(`SELECT 1; SELECT ? AS two`, [1]);
// Error: It looks like you tried to execute multiple statements with ";" while also using bind parameters. Many database APIs, including Execute Program's, don't allow you to do both of those at the same time. Try executing each statement in a separate call to "exec" instead of using semicolons. (This error is specific to Execute Program; it doesn't come from SQLite.)
```